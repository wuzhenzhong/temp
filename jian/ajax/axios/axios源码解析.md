# axios源码解析

0.7092019.06.14 17:34:47字数 2272阅读 467

> Axios是近几年非常火的HTTP请求库，官网上介绍Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

### 主要有以下features

- 从浏览器中创建 [XMLHttpRequests](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FXMLHttpRequest)
- 从 node.js 创建 [http](https://links.jianshu.com/go?to=http%3A%2F%2Fnodejs.org%2Fapi%2Fhttp.html) 请求
- 支持 [Promise](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FPromise) API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 [XSRF](https://links.jianshu.com/go?to=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FCross-site_request_forgery)

#### 一、那么首先了解下AXIOS为什么它可以在浏览器端和NodeJS环境中使用？

在axios中，使用适配器设计模式来屏蔽平台的差异性，让使用者可以在浏览器端和NodeJS环境中使用同一套API发起http请求。
http请求适配器（其实就是一个方法）

在axios项目里，http请求适配器主要指两种：XHR、http。
XHR的核心是浏览器端的XMLHttpRequest对象，
http核心是node的http[s].request方法。当然，axios也留给了用户通过config自行配置适配器的接口的，
不过，一般情况下，这两种适配器就能够满足从浏览器端向服务端发请求或者从node的http客户端向服务端发请求的需求。

axios的默认配置里的adapter是通过getDefaultAdapter()方法来获取的，它的逻辑如下：

```javascript
function getDefaultAdapter() {
  var adapter;
  // Only Node.JS has a process variable that is of [[Class]] process
  if (typeof process !== 'undefined' && Object.prototype.toString.call(process) === '[object process]') {
    // For node use HTTP adapter
    adapter = require('./adapters/http');
  } else if (typeof XMLHttpRequest !== 'undefined') {
    // For browsers use XHR adapter
    adapter = require('./adapters/xhr');
  }
  return adapter;
}
node要使用 HTTP 服务器和客户端，必须 require('http')。
```

#### 二、[源码分析](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Faxios%2Faxios%2Ftree%2Fmaster%2Flib)- axios为何会有多种使用方式？

- 第1种使用方式：axios(option)

```javascript
axios({
  url,
  method,
  headers,
})
```

- 第2种使用方式：axios(url[, option])

```javascript
axios(url, {
  method,
  headers,
})
```

- 第3种使用方式（对于get、delete等方法）：axios[method](url[, option])

```javascript
axios.get(url, {
  headers,
})
```

- 第4种使用方式（对于post、put等方法）：axios[method](url[, data[, option]])

```javascript
axios.post(url, data, {
  headers,
})
```

- 第5种使用方式：axios.request(option)

```javascript
axios.request({
  url,
  method,
  headers,
})
```

能够实现axios的多种使用方式的核心是createInstance方法
[https://github.com/axios/axios/blob/master/lib/axios.js](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Faxios%2Faxios%2Fblob%2Fmaster%2Flib%2Faxios.js)

```javascript
// /lib/axios.js
function createInstance(defaultConfig) {
  // 创建一个Axios实例
  var context = new Axios(defaultConfig);

  // 以下代码也可以这样实现：var instance = Axios.prototype.request.bind(context);
  // 这样instance就指向了request方法，且上下文指向context，所以可以直接以 instance(option) 方式调用 
  // Axios.prototype.request 内对第一个参数的数据类型判断，使我们能够以 instance(url, option) 方式调用
  var instance = bind(Axios.prototype.request, context);

  // 把Axios.prototype上的方法扩展到instance对象上，
  // 这样 instance 就有了 get、post、put等方法
  // 并指定上下文为context，这样执行Axios原型链上的方法时，this会指向context
  utils.extend(instance, Axios.prototype, context);

  // 把context对象上的自身属性和方法扩展到instance上
  // 注：因为extend内部使用的forEach方法对对象做for in 遍历时，只遍历对象本身的属性，而不会遍历原型链上的属性
  // 这样，instance 就有了  defaults、interceptors 属性。（这两个属性后面我们会介绍）
  utils.extend(instance, context);

  return instance;
}

// 接收默认配置项作为参数（后面会介绍配置项），创建一个Axios实例，最终会被作为对象导出
var axios = createInstance(defaults);
```

Axios是axios包的核心，一个Axios实例就是一个axios应用，其他方法都是对Axios内容的扩展
而Axios构造函数的核心方法是request方法，各种axios的调用方式最终都是通过request方法发请求的

```javascript
// /lib/core/Axios.js
function Axios(instanceConfig) {
  this.defaults = instanceConfig;
  this.interceptors = {
    request: new InterceptorManager(),
    response: new InterceptorManager()
  };
}

Axios.prototype.request = function request(config) {
  // ...省略代码
};

// 为支持的请求方法提供别名
utils.forEach(['delete', 'get', 'head', 'options'], function forEachMethodNoData(method) {
  Axios.prototype[method] = function(url, config) {
    return this.request(utils.merge(config || {}, {
      method: method,
      url: url
    }));
  };
});
utils.forEach(['post', 'put', 'patch'], function forEachMethodWithData(method) {
  Axios.prototype[method] = function(url, data, config) {
    return this.request(utils.merge(config || {}, {
      method: method,
      url: url,
      data: data
    }));
  };
});
```

通过以上代码，我们就可以以多种方式发起http请求了: axios()、axios.get()、axios.post()

一般情况，项目使用默认导出的axios实例就可以满足需求了，
如果不满足需求需要创建新的axios实例，axios包也预留了接口，
看下面的代码：

```javascript
// /lib/axios.js  -  31行
axios.Axios = Axios;
axios.create = function create(instanceConfig) {
  return createInstance(utils.merge(defaults, instanceConfig));
};
```

在开始说Axios.prototype.request之前，我们先来捋一捋在axios项目中，用户配置的config是怎么起作用的？

#### 三、用户配置的config是怎么起作用的？

这里说的config，指的是贯穿整个项目的配置项对象，
通过这个对象，可以设置：

http请求适配器、请求地址、请求方法、请求头header、 请求数据、请求或响应数据的转换、请求进度、http状态码验证规则、超时、取消请求等

可以发现，几乎axios所有的功能都是通过这个对象进行配置和传递的，
既是axios项目内部的沟通桥梁，也是用户跟axios进行沟通的桥梁。

首先我们看看，用户能以什么方式定义配置项:

```javascript
import axios from 'axios'

// 第1种：直接修改Axios实例上defaults属性，主要用来设置通用配置
axios.defaults[configName] = value;

// 第2种：发起请求时最终会调用Axios.prototype.request方法，然后传入配置项，主要用来设置“个例”配置
axios({
  url,
  method,
  headers,
})

// 第3种：新建一个Axios实例，传入配置项，此处设置的是通用配置
let newAxiosInstance = axios.create({
  [configName]: value,
})
```

看下 Axios.prototype.request 方法里的一行代码: (/lib/core/Axios.js - 第35行)

```javascript
config = utils.merge(defaults, {method: 'get'}, this.defaults, config);
```

可以发现此处将默认配置对象defaults（/lib/defaults.js）、Axios实例属性this.defaults、request请求的参数config进行了合并。

由此得出，多处配置的优先级由低到高是：

```javascript
—> 默认配置对象defaults（/lib/defaults.js)
—> { method: 'get' }
—> Axios实例属性this.defaults
—> request请求的参数config
```

留给大家思考一个问题: defaults 和 this.defaults 什么时候配置是相同的，什么时候是不同的？

至此，我们已经得到了将多处merge后的config对象，那么这个对象在项目中又是怎样传递的呢？

```javascript
Axios.prototype.request = function request(config) {
  // ...
  config = utils.merge(defaults, {method: 'get'}, this.defaults, config);

  var chain = [dispatchRequest, undefined];
  // 将config对象当作参数传给Primise.resolve方法
  var promise = Promise.resolve(config);
  // ...省略代码

  while (chain.length) {
    // config会按序通过 请求拦截器 - dispatchRequest方法 - 响应拦截器
    // 关于拦截器 和 dispatchRequest方法，下面会作为一个专门的小节来介绍。
    promise = promise.then(chain.shift(), chain.shift());
  }

  return promise;
};
```

至此，config走完了它传奇的一生 -_-
下一节就要说到重头戏了: Axios.prototype.request
所以只需对chain数组有个简单的了解就好，涉及到的[拦截器](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.imooc.com%2Farticle%2F32292%3Fblock_id%3Dtuijian_wz%23%E5%A6%82%E4%BD%95%E6%8B%A6%E6%88%AA%E8%AF%B7%E6%B1%82%E5%93%8D%E5%BA%94%E5%B9%B6%E4%BF%AE%E6%94%B9%E8%AF%B7%E6%B1%82%E5%8F%82%E6%95%B0%E4%BF%AE%E6%94%B9%E5%93%8D%E5%BA%94%E6%95%B0%E6%8D%AE)、[`dispatchRequest`]后面都会详细介绍
chain数组是用来盛放拦截器方法和dispatchRequest方法的，
通过promise从chain数组里按序取出回调函数逐一执行，最后将处理后的新的promise在Axios.prototype.request方法里返回出去，
并将response或error传送出去，这就是Axios.prototype.request的使命了

#### 四、拦截器是如何的实现?

使用方法

```javascript
// 添加请求拦截器
const myRequestInterceptor = axios.interceptors.request.use(config => {
    // 在发送http请求之前做些什么
    return config; // 有且必须有一个config对象被返回
}, error => {
    // 对请求错误做些什么
    return Promise.reject(error);
});

// 添加响应拦截器
axios.interceptors.response.use(response => {
  // 对响应数据做点什么
  return response; // 有且必须有一个response对象被返回
}, error => {
  // 对响应错误做点什么
  return Promise.reject(error);
});

// 移除某次拦截器
axios.interceptors.request.eject(myRequestInterceptor);
```

##### 思考

- 是否可以直接 return error？

```javascript
axios.interceptors.request.use(config => config, error => {
  // 是否可以直接 return error ？
  return Promise.reject(error); 
});
```

- 如何实现promise的链式调用

```javascript
new People('whr').sleep(3000).eat('apple').sleep(5000).eat('durian');

// 打印结果
// (等待3s)--> 'whr eat apple' -(等待5s)--> 'whr eat durian'
```

每个axios实例都有一个interceptors实例属性，
interceptors对象上有两个属性request、response。

```jsx
function Axios(instanceConfig) {
  // ...
  this.interceptors = {
    request: new InterceptorManager(),
    response: new InterceptorManager()
  };
}
```

这两个属性都是一个`InterceptorManager`实例，而这个`InterceptorManager`构造函数就是用来管理拦截器的。

我们先来看看`InterceptorManager`构造函数：

`InterceptorManager`构造函数就是用来实现拦截器的，这个构造函数原型上有3个方法：use、eject、forEach。
关于源码，其实是比较简单的，都是用来操作该构造函数的handlers实例属性的。

```jsx
// /lib/core/InterceptorManager.js

function InterceptorManager() {
  this.handlers = []; // 存放拦截器方法，数组内每一项都是有两个属性的对象，两个属性分别对应成功和失败后执行的函数。
}

// 往拦截器里添加拦截方法
InterceptorManager.prototype.use = function use(fulfilled, rejected) {
  this.handlers.push({
    fulfilled: fulfilled,
    rejected: rejected
  });
  return this.handlers.length - 1;
};

// 用来注销指定的拦截器
InterceptorManager.prototype.eject = function eject(id) {
  if (this.handlers[id]) {
    this.handlers[id] = null;
  }
};

// 遍历this.handlers，并将this.handlers里的每一项作为参数传给fn执行
InterceptorManager.prototype.forEach = function forEach(fn) {
  utils.forEach(this.handlers, function forEachHandler(h) {
    if (h !== null) {
      fn(h);
    }
  });
};
```

那么当我们通过`axios.interceptors.request.use`添加拦截器后，
axios内部又是怎么让这些拦截器能够在请求前、请求后拿到我们想要的数据的呢？

先看下代码：

```jsx
// /lib/core/Axios.js
Axios.prototype.request = function request(config) {
  // ...
  var chain = [dispatchRequest, undefined];

  // 初始化一个promise对象，状态微resolved，接收到的参数微config对象
  var promise = Promise.resolve(config);

  // 注意：interceptor.fulfilled 或 interceptor.rejected 是可能为undefined
  this.interceptors.request.forEach(function unshiftRequestInterceptors(interceptor) {
    chain.unshift(interceptor.fulfilled, interceptor.rejected);
  });

  this.interceptors.response.forEach(function pushResponseInterceptors(interceptor) {
    chain.push(interceptor.fulfilled, interceptor.rejected);
  });

  // 添加了拦截器后的chain数组大概会是这样的：
  // [
  //   requestFulfilledFn, requestRejectedFn, ..., 
  //   dispatchRequest, undefined,
  //   responseFulfilledFn, responseRejectedFn, ....,
  // ]

  // 只要chain数组长度不为0，就一直执行while循环
  while (chain.length) {
    // 数组的 shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。
    // 每次执行while循环，从chain数组里按序取出两项，并分别作为promise.then方法的第一个和第二个参数

    // 按照我们使用InterceptorManager.prototype.use添加拦截器的规则，正好每次添加的就是我们通过InterceptorManager.prototype.use方法添加的成功和失败回调

    // 通过InterceptorManager.prototype.use往拦截器数组里添加拦截器时使用的数组的push方法，
    // 对于请求拦截器，从拦截器数组按序读到后是通过unshift方法往chain数组数里添加的，又通过shift方法从chain数组里取出的，所以得出结论：对于请求拦截器，先添加的拦截器会后执行
    // 对于响应拦截器，从拦截器数组按序读到后是通过push方法往chain数组里添加的，又通过shift方法从chain数组里取出的，所以得出结论：对于响应拦截器，添加的拦截器先执行

    // 第一个请求拦截器的fulfilled函数会接收到promise对象初始化时传入的config对象，而请求拦截器又规定用户写的fulfilled函数必须返回一个config对象，所以通过promise实现链式调用时，每个请求拦截器的fulfilled函数都会接收到一个config对象

    // 第一个响应拦截器的fulfilled函数会接受到dispatchRequest（也就是我们的请求方法）请求到的数据（也就是response对象）,而响应拦截器又规定用户写的fulfilled函数必须返回一个response对象，所以通过promise实现链式调用时，每个响应拦截器的fulfilled函数都会接收到一个response对象

    // 任何一个拦截器的抛出的错误，都会被下一个拦截器的rejected函数收到，所以dispatchRequest抛出的错误才会被响应拦截器接收到。

    // 因为axios是通过promise实现的链式调用，所以我们可以在拦截器里进行异步操作，而拦截器的执行顺序还是会按照我们上面说的顺序执行，也就是 dispatchRequest 方法一定会等待所有的请求拦截器执行完后再开始执行，响应拦截器一定会等待 dispatchRequest 执行完后再开始执行。

    promise = promise.then(chain.shift(), chain.shift());

  }

  return promise;
};
```

现在，你应该已经清楚了拦截器是怎么回事，以及拦截器是如何在`Axios.prototype.request`方法里发挥作用的了，
那么处于"中游位置"的`dispatchRequest`是如何发送http请求的呢？

### dispatchrequest都做了哪些事

dispatchRequest主要做了3件事：
1，拿到config对象，对config进行传给http请求适配器前的最后处理；
2，http请求适配器根据config配置，发起请求
3，http请求适配器请求完成后，如果成功则根据header、data、和config.transformResponse（关于transformResponse，下面的[数据转换器](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.imooc.com%2Farticle%2F32292%3Fblock_id%3Dtuijian_wz%23%E6%95%B0%E6%8D%AE%E8%BD%AC%E6%8D%A2%E5%99%A8-%E8%BD%AC%E6%8D%A2%E8%AF%B7%E6%B1%82%E4%B8%8E%E5%93%8D%E5%BA%94%E6%95%B0%E6%8D%AE)会进行讲解）拿到数据转换后的response，并return。

```jsx
// /lib/core/dispatchRequest.js
module.exports = function dispatchRequest(config) {
  throwIfCancellationRequested(config);

  // Support baseURL config
  if (config.baseURL && !isAbsoluteURL(config.url)) {
    config.url = combineURLs(config.baseURL, config.url);
  }

  // Ensure headers exist
  config.headers = config.headers || {};

  // 对请求data进行转换
  config.data = transformData(
    config.data,
    config.headers,
    config.transformRequest
  );

  // 对header进行合并处理
  config.headers = utils.merge(
    config.headers.common || {},
    config.headers[config.method] || {},
    config.headers || {}
  );

  // 删除header属性里无用的属性
  utils.forEach(
    ['delete', 'get', 'head', 'post', 'put', 'patch', 'common'],
    function cleanHeaderConfig(method) {
      delete config.headers[method];
    }
  );

  // http请求适配器会优先使用config上自定义的适配器，没有配置时才会使用默认的XHR或http适配器，不过大部分时候，axios提供的默认适配器是能够满足我们的
  var adapter = config.adapter || defaults.adapter;

  return adapter(config).then(/**/);
};
```

### axios是如何用promise搭起基于xhr的异步桥梁的

axios是如何通过Promise进行异步处理的？

#### 如何使用

```jsx
import axios from 'axios'

axios.get(/**/)
.then(data => {
  // 此处可以拿到向服务端请求回的数据
})
.catch(error => {
  // 此处可以拿到请求失败或取消或其他处理失败的错误对象
})
```

#### 源码分析

先来一个图简单的了解下axios项目里，http请求完成后到达用户的顺序流：



![img](https://upload-images.jianshu.io/upload_images/2790249-3834037cb67425f0.png?imageMogr2/auto-orient/strip|imageView2/2/w/940/format/webp)

axioshttp请求完成后达到的用户顺序流.png

通过[axios为何会有多种使用方式](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.imooc.com%2Farticle%2F32292%3Fblock_id%3Dtuijian_wz%23axios%E4%B8%BA%E4%BD%95%E4%BC%9A%E6%9C%89%E5%A4%9A%E7%A7%8D%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F)我们知道，
用户无论以什么方式调用axios，最终都是调用的`Axios.prototype.request`方法，
这个方法最终返回的是一个Promise对象。

```jsx
Axios.prototype.request = function request(config) {
  // ...
  var chain = [dispatchRequest, undefined];
  // 将config对象当作参数传给Primise.resolve方法
  var promise = Promise.resolve(config);

  while (chain.length) {
    promise = promise.then(chain.shift(), chain.shift());
  }

  return promise;
};
```

`Axios.prototype.request`方法会调用`dispatchRequest`方法，而`dispatchRequest`方法会调用`xhrAdapter`方法，`xhrAdapter`方法返回的是还一个Promise对象

```jsx
// /lib/adapters/xhr.js
function xhrAdapter(config) {
  return new Promise(function dispatchXhrRequest(resolve, reject) {
    // ... 省略代码
  });
};
```

`xhrAdapter`内的XHR发送请求成功后会执行这个Promise对象的`resolve`方法,并将请求的数据传出去,
反之则执行`reject`方法，并将错误信息作为参数传出去。

```jsx
// /lib/adapters/xhr.js
var request = new XMLHttpRequest();
var loadEvent = 'onreadystatechange';

request[loadEvent] = function handleLoad() {
  // ...
  // 往下走有settle的源码
  settle(resolve, reject, response);
  // ...
};
request.onerror = function handleError() {
  reject(/**/);
  request = null;
};
request.ontimeout = function handleTimeout() {
  reject(/**/);
  request = null;
};
```

验证服务端的返回结果是否通过验证：

```jsx
// /lib/core/settle.js
module.exports = function settle(resolve, reject, response) {
  var validateStatus = response.config.validateStatus;
  if (!validateStatus || validateStatus(response.status)) {
    resolve(response);
  } else {
    reject(createError(
      'Request failed with status code ' + response.status,
      response.config,
      null,
      response.request,
      response
    ));
  }
};
```

回到`dispatchRequest`方法内，首先得到`xhrAdapter`方法返回的Promise对象,
然后通过`.then`方法，对`xhrAdapter`返回的Promise对象的成功或失败结果再次加工，
成功的话，则将处理后的`response`返回，
失败的话，则返回一个状态为`rejected`的Promise对象，

```jsx
  return adapter(config).then(function onAdapterResolution(response) {
    // ...
    return response;
  }, function onAdapterRejection(reason) {
    // ...
    return Promise.reject(reason);
  });
};
```

那么至此，用户调用`axios()`方法时，就可以直接调用Promise的`.then`或`.catch`进行业务处理了。

#### 五、如何取消已经发送的请求？

如何使用

```jsx
import axios from 'axios'

// 第一种取消方法
axios.get(url, {
  cancelToken: new axios.CancelToken(cancel => {
    if (/* 取消条件 */) {
      cancel('取消日志');
    }
  })
});

// 第二种取消方法
const CancelToken = axios.CancelToken;
const source = CancelToken.source();
axios.get(url, {
  cancelToken: source.token
});
source.cancel('取消日志');
```

复制代码源码分析

```jsx
// /cancel/CancelToken.js  -  11行
function CancelToken(executor) {
 
  var resolvePromise;
  this.promise = new Promise(function promiseExecutor(resolve) {
    resolvePromise = resolve;
  });
  var token = this;
  executor(function cancel(message) {
    if (token.reason) {
      return;
    }
    token.reason = new Cancel(message);
    resolvePromise(token.reason);
  });
}

// /lib/adapters/xhr.js  -  159行
if (config.cancelToken) {
    config.cancelToken.promise.then(function onCanceled(cancel) {
        if (!request) {
            return;
        }
        request.abort();
        reject(cancel);
        request = null;
    });
}

复制代码取消功能的核心是通过CancelToken内的this.promise = new Promise(resolve => resolvePromise = resolve)，
得到实例属性promise，此时该promise的状态为pending
通过这个属性，在/lib/adapters/xhr.js文件中继续给这个promise实例添加.then方法
（xhr.js文件的159行config.cancelToken.promise.then(message => request.abort())）；
在CancelToken外界，通过executor参数拿到对cancel方法的控制权，
这样当执行cancel方法时就可以改变实例的promise属性的状态为rejected，
从而执行request.abort()方法达到取消请求的目的。
上面第二种写法可以看作是对第一种写法的完善，
因为很多是时候我们取消请求的方法是用在本次请求方法外，
例如，发送A、B两个请求，当B请求成功后，取消A请求。

// 第1种写法：
let source;
axios.get(Aurl, {
  cancelToken: new axios.CancelToken(cancel => {
    source = cancel;
  })
});
axios.get(Burl)
.then(() => source('B请求成功了'));

// 第2种写法：
const CancelToken = axios.CancelToken;
const source = CancelToken.source();
axios.get(Aurl, {
  cancelToken: source.token
});
axios.get(Burl)
.then(() => source.cancel('B请求成功了'));
```

复制代码相对来说，我更推崇第1种写法，因为第2种写法太隐蔽了，不如第一种直观好理解。
[https://codepen.io/milletChen/pen/rEVJbG](https://links.jianshu.com/go?to=https%3A%2F%2Fcodepen.io%2FmilletChen%2Fpen%2FrEVJbG)

发现的问题

/lib/adapters/xhr.js文件中，onCanceled方法的参数不应该叫message么，为什么叫cancel？

/lib/adapters/xhr.js文件中，onCanceled方法里，reject里应该将config信息也传出来

#### 六、扩展[如果服务器或者网络不稳定掉包了, 你们该如何处理呢?](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.v2ex.com%2Ft%2F460436)

#### 总结

axios这个项目里，有很多对JS使用很巧妙的地方，比如对promise的串联操作（当然你也可以说这块是借鉴很多异步中间件的处理方式）,让我们可以很方便对请求前后的各种处理方法的流程进行控制；很多实用的小优化，比如请求前后的数据处理，省了程序员一遍一遍去写JSON.xxx了；同时支持了浏览器和node两种环境，对使用node的项目来说无疑是极好的。