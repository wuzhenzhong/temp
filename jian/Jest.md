# Jest

[![img](https://upload.jianshu.io/users/upload_avatars/9478188/cb5115d8-8842-42f5-b364-b1668ed9e636.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/db6a3e63d605)

[我向你奔](https://www.jianshu.com/u/db6a3e63d605)关注

12018.08.06 17:23:29字数 630阅读 37,005

jest 是facebook推出的一款测试框架，集成了 Mocha，chai，jsdom，sinon等功能。

#### 安装与配置



```undefined
npm install --save-dev jest
npm install -g jest
```

运行命令 jest 后会自动运行项目下所有*.test.js和*.spec.js这种格式的文件。涉及到运用 ES 或 react 的，要与babel相结合，加上.babelrc文件即可。jest的配置默认只需要在package.json中配置即可，当然也可以用独立的配置文件。

我们这里直接将 jest 的运行范围限定在test文件夹下，而不是全部，所以在package.json中加入如下配置：



```bash
 "jest": {
    "testRegex": "/test/.*.test.jsx?$"
 }
```

注意到这里的匹配规则是正则表达式

#### 基本用法

和之前介绍的 mocha 和 chai 的功能很像，甚至可以兼容部分 mocha 和 chai 的语法。可以这么写



```tsx
import React from 'react'
import { shallow } from 'enzyme'
import CommentItem from './commentItem'

describe('测试评论列表项组件', () => {
  // 这是mocha的玩法，jest可以直接兼容
  it('测试评论内容小于等于200时不出现展开收起按钮', () => {
    const propsData = {
      name: 'hj',
      content: '测试标题'
    }
    const item = shallow(<CommentItem {...propsData} />)
    // 这里的断言实际上和chai的expect是很像的
    expect(item.find('.btn-expand').length).toBe(0);
  })
  // 这是jest的玩法，推荐用这种
  test('两数相加结果为两个数字的和', () => {
    expect(3).toBe(3);
  });
}
```

#### jest与eslint检测

如果看了上面的代码会发现我没有引用任何类似于



```jsx
import *  from 'jest'
```

的代码，而那个expect是没有定义的。
这段代码直接运行**jest**命令没有任何问题，但是eslint会检测出错，对于这种情况，我们可以再eslint配置文件.eslintrc中加入以下代码：



```bash
"env": {
    "jest": true
  },
```

#### jest的断言

jest有自己的断言玩法。除了前面的代码中已经写到的



```kotlin
expect(x).toBe(y)//判断相等，使用Object.is实现
```

还有常用的



```kotlin
expect({a:1}).toBe({a:1})//判断两个对象是否相等
expect(1).not.toBe(2)//判断不等
expect(n).toBeNull(); //判断是否为null
expect(n).toBeUndefined(); //判断是否为undefined
expect(n).toBeDefined(); //判断结果与toBeUndefined相反
expect(n).toBeTruthy(); //判断结果为true
expect(n).toBeFalsy(); //判断结果为false
expect(value).toBeGreaterThan(3); //大于3
expect(value).toBeGreaterThanOrEqual(3.5); //大于等于3.5
expect(value).toBeLessThan(5); //小于5
expect(value).toBeLessThanOrEqual(4.5); //小于等于4.5
expect(value).toBeCloseTo(0.3); // 浮点数判断相等
expect('Christoph').toMatch(/stop/); //正则表达式判断
expect(['one','two']).toContain('one'); //不解释

function compileAndroidCode() {
  throw new ConfigError('you are using the wrong JDK');
}

test('compiling android goes as expected', () => {
  expect(compileAndroidCode).toThrow();
  expect(compileAndroidCode).toThrow(ConfigError); //判断抛出异常
}）
```

[更多断言玩法](https://facebook.github.io/jest/docs/en/expect.html)

#### jest的 mock

介绍了jest替代mocha和chai的部分，那么接下来就看看如何替代sinon。
下面是官网的示例：



```jsx
function forEach(items, callback) {
  for (let index = 0; index < items.length; index++) {
   callback(items[index]);
  }
}

const mockCallback = jest.fn();
forEach([0, 1], mockCallback);

// 判断是否被执行两次
expect(mockCallback.mock.calls.length).toBe(2);

// 判断函数被首次执行的第一个形参为0
expect(mockCallback.mock.calls[0][0]).toBe(0);

// 判断函数第二次被执行的第一个形参为1
expect(mockCallback.mock.calls[1][0]).toBe(1);
```

从上面可以看到这种玩法很类似于sinon的 sinon.spy()。当然也有类似于stub返回值的那种玩法，更多的请参考 [jest mock的更多玩法](https://facebook.github.io/jest/docs/en/mock-functions.html)

#### mock文件和css module的问题

如果js文件中引用了css或者本地其他文件，那么就可能测试失败。为了解决这个问题，同时也为了提高测试效率：



```bash
"jest": {
    "moduleNameMapper": {
     "\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga)$": "<rootDir>/test/config/fileMock.js",
     "\\.(css|less)$": "identity-obj-proxy"
 }
```

而fileMock.js文件内容为：



```java
module.exports = 'test-file-stub';
```

然后安装identity-obj-proxy即可：



```undefined
npm install --save-dev identity-obj-proxy
```

#### jest与别名

在webpack中经常会用到别名，而jest测试时，如果文件中引用了别名会出现找不到文件的问题。毕竟jest测试时没有经过webpack处理。对于以下玩法



```css
resolve: {  
    alias: {  
        common: path.resolve(__dirname, 'plugins/common/')  
    }  
} 
```

可以通过



```bash
"jest": {
    "testRegex": "./src/test/.*.test.js$",
    "moduleNameMapper": {
      "^common(.*)$": "<rootDir>/plugins/common$1",
    }
}
```

这个和之前 mock文件和css module的问题 一样，都是使用了moduleNameMapper这个属性

#### 生成测试覆盖率报告

只需要在jest命令后加入 --coverage即可



```undefined
jest --coverage
```



![img](https://upload-images.jianshu.io/upload_images/9478188-c537c433f99d3e55?imageMogr2/auto-orient/strip|imageView2/2/w/833/format/webp)





14人点赞



[前端单元测试](https://www.jianshu.com/nb/27965365)