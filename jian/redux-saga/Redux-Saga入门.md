# Redux-Saga入门

12018.07.21 16:42:24字数 3249阅读 1340

## 1. redux-thunk处理副作用的缺点

### 1.1 redux的副作用处理

redux中的数据流大致是：

**UI——>action(plain)——>reducer——>state——>UI**



![img](https://upload-images.jianshu.io/upload_images/3817318-a94569eb81985bd7.png?imageMogr2/auto-orient/strip|imageView2/2/w/573/format/webp)

redux数据流



redux遵循函数式编程的规则，上述的数据流中，action是一个原始js对象(plain object)且reducer是一个纯函数，对于同步且**没有副作用**的操作，上述的数据流起到可以管理数据，从而控制视图层更新的目的。

**但是如果存在副作用，比如ajax异步请求等等，那么应该怎么做？**

如果存在副作用函数，那么我们需要首先处理副作用函数，然后生成原始的js对象。如何处理副作用操作，**在redux中选择在发出action，到reducer处理函数之间使用中间件处理副作用**。

redux增加中间件处理副作用后的数据流大致如下：

**UI——>action(side function)——>middleware——>action(plain)——>reducer——>state——>UI**



![img](https://upload-images.jianshu.io/upload_images/3817318-b5283f4ca8f39db9.png?imageMogr2/auto-orient/strip|imageView2/2/w/719/format/webp)

有副作用的数据流

在有副作用的action和原始的action之间增加中间件处理，中间件的作用就是：**转换异步操作，生成原始的action，这样，reducer函数就能处理相应的action，从而改变state，更新UI。**

### 1.2 redux-thunk

redux-thunk是redux作者给出的中间件，实现极为简单，10多行代码：

```jsx
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

这几行代码做的事情就是判别action的类型，如果action是函数，就调用这个函数，调用的步骤为：

```undefined
action(dispatch, getState, extraArgument);
```

实参为dispatch和getState，因此我们在定义action为thunk函数是，一般形参为dispatch和getState。

### 1.3 redux-thunk的缺点

redux-thunk的缺点也是很明显的，redux-thunk仅仅做了执行这个函数，并不在乎函数主体内是什么，也就是说redux-thunk使得redux可以接受函数作为action，但是函数的内部可以多种多样。比如下面是一个获取商品列表的异步操作所对应的action：

```tsx
export default () => dispatch => {
    // fecth返回的是一个promise
    fetch('/api/goodList', {
      method: 'GET',
      dataType: 'json',
    }).then(res => {
      const data=JSON.parse(res).data;
      if(json.msg==200) {
        dispatch({ type: 'INIT', data });
      }
    }, error  => {
      console.log(error);
    });
};
```

从这个具有副作用的action中，可以看出，函数内部极为复杂。如果需要为每一个异步操作都如此定义一个action，显然action不易维护：

- action的形式不统一
- 异步操作太为分散，分散在了各个action中

## 2. redux-saga写一个hellosaga

跟redux-thunk相比redux-saga是控制执行的generator，在redux-saga中action是原始的js对象，把所有的异步副作用操作放在了saga函数里面。这样既统一了action的形式，又使得异步操作集中可以被集中处理。redux-saga是通过genetator实现的，如果不支持generator需要通过插件babel-polyfill转义。

### 2.1 创建一个helloSaga.js文件

```jsx
export function * helloSaga() {
  console.log('Hello Sagas!');
}
```

### 2.2 在redux中使用redux-saga中间件

在main.js中：

```jsx
import { createStore, applyMiddleware } from 'redux'；
import createSagaMiddleware from 'redux-saga'；
import { helloSaga } from './sagas'；
const sagaMiddleware=createSagaMiddleware();
const store = createStore(
 reducer,
 applyMiddleware(sagaMiddleware)
);
sagaMiddleware.run(helloSaga); // Hello, Sagas!
```

和调用redux的其他中间件一样，如果想使用redux-saga中间件，那么只要在applyMiddleware中调用一个createSagaMiddleware的实例。唯一不同的是需要调用run方法使得generator可以开始执行。

## 2.3 redux-saga的使用技术细节

redux-saga除了上述的action统一、可以集中处理异步操作等优点外，redux-saga中使用声明式的Effect以及提供了更加细腻的控制流。

### 2.3.1 声明式的Effect

redux-saga中最大的特点就是提供了声明式的Effect，声明式的Effect使得redux-saga监听原始js对象形式的action，并且可以方便单元测试。

- 首先，在redux-saga中提供了一系列的api，比如take、put、all、select等API ，在redux-saga中将这一系列的api都定义为Effect。这些Effect执行后，当函数resolve时返回一个描述对象，然后redux-saga中间件根据这个描述对象恢复执行generator中的函数。

首先来看redux-thunk的大体过程：

**action1(side function)——>redux-thunk监听——>执行相应的有副作用的方法—>action2(plain object)**



![img](https://upload-images.jianshu.io/upload_images/3817318-01858a98f27b21ad.png?imageMogr2/auto-orient/strip|imageView2/2/w/674/format/webp)

redux-thunk

转化到action2是一个原始js对象形式的action，然后执行reducer函数就会更新store中的state。

而redux-saga的大体过程如下：

**action1(plain object)——>redux-saga监听——>执行相应的Effect方法——>返回描述对象——>恢复执行异步和副作用函数——>action2(plain object)**





![img](https://upload-images.jianshu.io/upload_images/3817318-a140af46f67127c4.png?imageMogr2/auto-orient/strip|imageView2/2/w/954/format/webp)

[redux-saga](https://user-images.githubusercontent.com/17233651/42163582-c92ae632-7e35-11e8-804b-5e6bb60921da.png)



对比redux-thunk发现，redux-saga中监听到了原始js对象action，并不会马上执行副作用操作，会先通过Effect方法将其转化成一个描述对象，然后再将描述对象作为标识，再恢复执行副作用函数。

通过使用Effect类函数，可以方便单元测试，不需要测试副作用函数的返回结果。只需要比较执行Effect方法后返回的描述对象，与所期望的描述对象是否相同即可。

举例来说，call方法是一个Effect类方法：

```jsx
import { call } from 'redux-saga/effects'

function* fetchProducts() {
  const products = yield call(Api.fetch, '/products')
  // ...
}
```

上述代码中，比如我们需要测试Api.fetch返回的结果是否符合预期，通过调用call方法，返回一个描述对象。这个描述对象包含了所需要调用的方法和执行方法时的实际参数，我们认为只要描述对象相同，也就是说只要调用的方法和执行该方法时的实际参数相同，就认为最后执行的结果肯定是满足预期的，这样可以方便的进行单元测试，不需要模拟Api.fetch函数的具体返回结果。

```jsx
import { call } from 'redux-saga/effects'
import Api from '...'

const iterator = fetchProducts()

// expects a call instruction
assert.deepEqual(
  iterator.next().value,
  call(Api.fetch, '/products'),
  "fetchProducts should yield an Effect call(Api.fetch, './products')"
)
```

### 2.3.2 Effect提供的具体方法

下面来介绍几个Effect中常用的几个方法，从低阶的API，比如take，call(apply)，fork，put，select等，以及高阶API，比如takeEvery和takeLatest等，从而加深对redux-saga用法的认识
引入：

```csharp
import { take, call, put, select, fork, takeEvery, takeLatest } from 'redux-saga/effects'
```

- take
  take这个方法，是用来监听action，返回的是监听到的action对象。比如：

```go
const loginAction = {
   type:'login'
}
```

在UI Component中dispatch一个action:

```undefined
dispatch(loginAction)
```

在saga中使用：

```csharp
const action = yield take('login');
```

可以监听到UI传递到中间件的Action,上述take方法的返回，就是dispatch的原始对象。一旦监听到login动作，返回的action为：

```css
{
  type:'login'
}
```

- call(apply)

call和apply方法与js中的call和apply相似，以call方法为例：

```css
call(fn, ...args)
```

call方法调用fn，参数为args，返回一个描述对象。不过这里call方法传入的函数fn可以是普通函数，也可以是generator。call方法应用很广泛，在redux-saga中使用异步请求等常用call方法来实现。

```csharp
yield call(fetch, '/userInfo',username)
```

- put
  在前面提到，redux-saga做为中间件，工作流是这样的：
  **UI——>action1——>redux-saga中间件——>action2——>reducer..**

从工作流中，我们发现redux-saga执行完副作用函数后，必须发出action，然后这个action被reducer监听，从而达到更新state的目的。相应的这里的put对应与redux中的dispatch，工作流程图如下：





![img](https://upload-images.jianshu.io/upload_images/3817318-c84f4c8f5acc50b8.png?imageMogr2/auto-orient/strip|imageView2/2/w/751/format/webp)





从图中可以看出redux-saga执行副作用方法转化action时，put这个Effect方法跟redux原始的dispatch相似，都是可以发出action，且发出的action都会被reducer监听到。put的使用方法：

```css
 yield put({ type:'login' })
```

- select
  put方法与redux中的dispatch相对应，同样的如果我们想在中间件中获取state，那么需要使用select。select方法对应的是redux中的getState，用户获取store中的state，使用方法：

```csharp
const state= yield select()；
```

- fork
  fork方法相当于web work，fork方法不会阻塞主线程，在非阻塞调用中十分有用。
- takeEvery和takeLatest
  takeEvery和takeLatest用于监听相应的动作并执行相应的方法，是构建在take和fork上面的高阶api，比如要监听login动作，用takeEvery方法可以：

```bash
takeEvery('login', loginFunc);
```

takeEvery监听到login的动作，就会执行loginFunc方法，除此之外，takeEvery可以同时监听到多个相同的action。
takeLatest方法跟takeEvery是相同方式调用：

```bash
takeLatest('login', loginFunc);
```

与takeLatest不同的是，takeLatest是会监听执行最近的那个被触发的action。

## 2.3.4 redux-saga实现一个登陆和列表样例

接着实现一个redux-saga样例，存在一个登陆页，登陆成功后，显示列表页，并且，在列表页，可以点击登出，返回到登陆页。例子的最终展示效果如下：





![img](https://upload-images.jianshu.io/upload_images/3817318-cf3032a4b47c45bb.gif?imageMogr2/auto-orient/strip|imageView2/2/w/661/format/webp)

[login](https://user-images.githubusercontent.com/17233651/42272511-e0046232-7fb8-11e8-8219-e6f5c3af598c.gif)



样例的功能流程图为：





![img](https://upload-images.jianshu.io/upload_images/3817318-c5e167ff4cc1a187.png?imageMogr2/auto-orient/strip|imageView2/2/w/364/format/webp)





接着按照上述的流程来一步步的实现所对应的功能。

### 2.3.4.1 LoginPanel(登陆页)

登陆页的功能包括

- 输入时保存用户名
- 输入时保存密码
- 点击sign in 请求判断是否登陆成功

#### 1. 输入时保存用户名和密码

用户名输入框和密码框onchange时触发的函数为：

```tsx
 changeUsername:(e)=>{
    dispatch({ type:'CHANGE_USERNAME', value: e.target.value });
 },
changePassword:(e)=>{
  dispatch({ type:'CHANGE_PASSWORD', value: e.target.value });
}
```

在函数中最后会dispatch两个action：**CHANGE_USERNAME和CHANGE_PASSWORD**。

在saga.js文件中监听这两个action并执行副作用函数，最后put发出转化后的action，给reducer函数调用：

```csharp
function * watchUsername() {
  while(true){
    const action= yield take('CHANGE_USERNAME');
    yield put({ type: 'change_username', value: action.value });
  }
}
function * watchPassword() {
  while(true){
    const action=yield take('CHANGE_PASSWORD');
    yield put({ type: 'change_password', value: action.value });
  }
}
```

最后在reducer中接收到redux-saga的put方法传递过来的action: **change_username和change_password**，然后更新state。

#### 2. 监听登陆事件判断登陆是否成功

在UI中发出的登陆事件为：

```tsx
toLoginIn: (username,password)=>{
  dispatch({ type: 'TO_LOGIN_IN', username, password });
}
```

登陆事件的action为：TO_LOGIN_IN。对于登入事件的处理函数为：

```tsx
 while(true){
    // 监听登入事件
    const action1 = yield take('TO_LOGIN_IN');
    const res = yield call(fetchSmart, '/login', {
      method: 'POST',
      body: JSON.stringify({
        username: action1.username,
        password: action1.password
    })
    if (res) {
      put({ type:'to_login_in' });
    }
});
```

在上述的处理函数中，首先监听原始动作提取出传递来的用户名和密码，然后请求是否登陆成功，如果登陆成功有返回值，则执行put的action: to_login_in.

### 2.3.4.2 LoginSuccess(登陆成功列表展示页)

登陆成功后的页面功能包括：

- 获取列表信息，展示列表信息
- 登出功能，点击可以返回登陆页面

#### 1. 获取列表信息

```tsx
import { delay } from 'redux-saga';

function * getList() {
  try {
   yield delay(3000);
   const res = yield call(fetchSmart, '/list', {
     method: 'POST',
     body: JSON.stringify({})
   });
   yield put({ type: 'update_list', list: res.data.activityList });
 } catch(error) {
   yield put({ type: 'update_list_error', error });
 }
}
```

为了演示请求过程，我们在本地mock，通过redux-saga的工具函数delay，delay的功能相当于延迟xx秒，因为真实的请求存在延迟，因此可以用delay在本地模拟真实场景下的请求延迟。

#### 2. 登出功能

```csharp
const action2 = yield take('TO_LOGIN_OUT');
yield put({ type:'to_login_out' });
```

与登入相似，登出的功能从UI处接受action:TO_LOGIN_OUT，然后转发action:to_login_out。

### 3. 完整的实现登入登出和列表展示的代码

```csharp
function* getList () {
  try {
   yield delay(3000);
   const res = yield call(fetchSmart, '/list', {
     method: 'POST',
     body: JSON.stringify({})
   });
   yield put({ type:'update_list', list: res.data.activityList });
 } catch(error) {
   yield put({ type:'update_list_error', error });
 }
}

function * watchIsLogin () {
  while(true){
    // 监听登入事件
    const action1 = yield take('TO_LOGIN_IN');

    const res = yield call(fetchSmart, '/login', {
      method: 'POST',
      body: JSON.stringify({
        username: action1.username,
        password: action1.password
      })
    });

    // 根据返回的状态码判断登陆是否成功
    if(res.status === 10000){
      yield put({ type:'to_login_in' });
      // 登陆成功后获取首页的活动列表
      yield call(getList);
    }

    // 监听登出事件
    const action2 = yield take('TO_LOGIN_OUT');
    yield put({ type: 'to_login_out' });
  }
}
```

通过请求状态码判断登入是否成功，在登陆成功后，可以通过：`yield call(getList)`的方式调用获取活动列表的函数getList。这样乍一看没有什么问题，但是注意call方法调用是会阻塞主线程的，具体来说：

- 在call方法调用结束之前，call方法之后的语句是无法执行的
- 如果call(getList)存在延迟，call(getList)之后的语句 const action2=yieldtake('TO_LOGIN_OUT')在call方法返回结果之前无法执行
- 在延迟期间的登出操作会被忽略。

用框图可以更清楚的分析：



![img](https://upload-images.jianshu.io/upload_images/3817318-82e0dce37d36624b.png?imageMogr2/auto-orient/strip|imageView2/2/w/872/format/webp)

image.png

call方法调用阻塞主线程的具体效果如下动图所示：



![img](https://upload-images.jianshu.io/upload_images/3817318-80d49f57a5c33409.gif?imageMogr2/auto-orient/strip|imageView2/2/w/661/format/webp)

白屏时为请求列表的等待时间，在此时，我们点击登出按钮，无法响应登出功能，直到请求列表成功，展示列表信息后，点击登出按钮才有相应的登出功能。也就是说call方法阻塞了主线程。

### 2.3.4.3 无阻塞调用

我们在第二章中，介绍了fork方法可以类似与web work，fork方法不会阻塞主线程。应用于上述例子，我们可以将：

```csharp
yield call(getList)
```

修改为：

```csharp
yield fork(getList)
```

这样展示的结果为：



![img](https://upload-images.jianshu.io/upload_images/3817318-915fa328c8fe904e.gif?imageMogr2/auto-orient/strip|imageView2/2/w/661/format/webp)

通过fork方法不会阻塞主线程，在白屏时点击登出，可以立刻响应登出功能，从而返回登陆页面。

## 3. 总结

通过上述章节，我们可以概括出redux-saga做为redux中间件的全部优点：

- 统一action的形式，在redux-saga中，从UI中dispatch的action为原始对象
- 集中处理异步等存在副作用的逻辑
- 通过转化effects函数，可以方便进行单元测试
- 完善和严谨的流程控制，可以较为清晰的控制复杂的逻辑。