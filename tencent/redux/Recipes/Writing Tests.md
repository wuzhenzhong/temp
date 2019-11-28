# 编写测试( Writing Tests )

- 贡献者1人

  

因为你编写的大多数Redux代码都是函数，而且其中很多都是pure的，所以它们很容易测试而不需模拟。

### 设置

我们推荐将 [Jest ](http://facebook.github.io/jest/)作为测试引擎。请注意，它在 Node 环境中运行，因此您将无法访问 DOM 。

```javascript
npm install --save-dev jest
```

为了与 [Babel ](http://babeljs.io/)一起使用，您需要安装 `babel-jest `：

```javascript
npm install --save-dev babel-jest
```

然后将其配置为在`.babelrc`情况下使用 ES2015 功能：

```javascript
{
  "presets": ["es2015"]
}
```

然后，在您的`package.json`中将其添加到`scripts`：

```javascript
{
  ...
  "scripts": {
    ...
    "test": "jest",
    "test:watch": "npm test -- --watch"
  },
  ...
}
```

并运行一次`npm test`，或者用`npm run test:watch`测试每个文件的更改。

### Action Creators

在 Redux 中，action creators 是返回普通对象的函数。测试 action creators 时，我们要测试是否调用了正确的 action creator ，以及是否返回了正确的动作。

#### 示例

```javascript
export function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}
```

测试如下：

```javascript
import * as actions from '../../actions/TodoActions'
import * as types from '../../constants/ActionTypes'

describe('actions', () => {
  it('should create an action to add a todo', () => {
    const text = 'Finish docs'
    const expectedAction = {
      type: types.ADD_TODO,
      text
    }
    expect(actions.addTodo(text)).toEqual(expectedAction)
  })
})
```

### Async Action Creators

对于使用 [Redux Thunk ](https://github.com/gaearon/redux-thunk)或其他中间件的异步操作创建者，最好完全模拟 Redux 存储进行测试。您可以使用 [redux-mock-store ](https://github.com/arnaudbenard/redux-mock-store)将中间件应用到[模拟商店](https://github.com/arnaudbenard/redux-mock-store)。你也可以使用 [nock ](https://github.com/pgte/nock)来模拟 HTTP 请求。

#### 示例

```javascript
import fetch from 'isomorphic-fetch'

function fetchTodosRequest() {
  return {
    type: FETCH_TODOS_REQUEST
  }
}

function fetchTodosSuccess(body) {
  return {
    type: FETCH_TODOS_SUCCESS,
    body
  }
}

function fetchTodosFailure(ex) {
  return {
    type: FETCH_TODOS_FAILURE,
    ex
  }
}

export function fetchTodos() {
  return dispatch => {
    dispatch(fetchTodosRequest())
    return fetch('http://example.com/todos')
      .then(res => res.json())
      .then(json => dispatch(fetchTodosSuccess(json.body)))
      .catch(ex => dispatch(fetchTodosFailure(ex)))
  }
}
```

测试如下：

```javascript
import configureMockStore from 'redux-mock-store'
import thunk from 'redux-thunk'
import * as actions from '../../actions/TodoActions'
import * as types from '../../constants/ActionTypes'
import nock from 'nock'
import expect from 'expect' // You can use any testing library

const middlewares = [thunk]
const mockStore = configureMockStore(middlewares)

describe('async actions', () => {
  afterEach(() => {
    nock.cleanAll()
  })

  it('creates FETCH_TODOS_SUCCESS when fetching todos has been done', () => {
    nock('http://example.com/')
      .get('/todos')
      .reply(200, { body: { todos: ['do something'] } })

    const expectedActions = [
      { type: types.FETCH_TODOS_REQUEST },
      { type: types.FETCH_TODOS_SUCCESS, body: { todos: ['do something'] } }
    ]
    const store = mockStore({ todos: [] })

    return store.dispatch(actions.fetchTodos()).then(() => {
      // return of async actions
      expect(store.getActions()).toEqual(expectedActions)
    })
  })
})
```

### Reducers

在将动作应用到之前的状态之后，reducer 应该返回新的状态，这就是下面测试的行为。

#### 示例

```javascript
import { ADD_TODO } from '../constants/ActionTypes'

const initialState = [
  {
    text: 'Use Redux',
    completed: false,
    id: 0
  }
]

export default function todos(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        {
          id: state.reduce((maxId, todo) => Math.max(todo.id, maxId), -1) + 1,
          completed: false,
          text: action.text
        },
        ...state
      ]

    default:
      return state
  }
}
```

测试如下：

```javascript
import reducer from '../../reducers/todos'
import * as types from '../../constants/ActionTypes'

describe('todos reducer', () => {
  it('should return the initial state', () => {
    expect(reducer(undefined, {})).toEqual([
      {
        text: 'Use Redux',
        completed: false,
        id: 0
      }
    ])
  })

  it('should handle ADD_TODO', () => {
    expect(
      reducer([], {
        type: types.ADD_TODO,
        text: 'Run the tests'
      })
    ).toEqual([
      {
        text: 'Run the tests',
        completed: false,
        id: 0
      }
    ])

    expect(
      reducer(
        [
          {
            text: 'Use Redux',
            completed: false,
            id: 0
          }
        ],
        {
          type: types.ADD_TODO,
          text: 'Run the tests'
        }
      )
    ).toEqual([
      {
        text: 'Run the tests',
        completed: false,
        id: 1
      },
      {
        text: 'Use Redux',
        completed: false,
        id: 0
      }
    ])
  })
})
```

### 组件

React 组件的一个好处是它们通常很小，只能依靠它们的道具。这使他们很容易测试。

首先，我们将安装 [Enzyme ](http://airbnb.io/enzyme/)。Enzyme 使用下面的 [React Test Utilities ](https://facebook.github.io/react/docs/test-utils.html)，但更方便，可读且功能强大。

```javascript
npm install --save-dev enzyme
```

为了测试这些组件，我们制作了一个`setup()`帮助器，它将道具回调作为道具传递，并用 [shallow rendering ](http://airbnb.io/enzyme/docs/api/shallow.html)组件。这让单个测试可以确定回调是否在预期时被调用。

#### 示例

```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import TodoTextInput from './TodoTextInput'

class Header extends Component {
  handleSave(text) {
    if (text.length !== 0) {
      this.props.addTodo(text)
    }
  }

  render() {
    return (
      <header className="header">
        <h1>todos</h1>
        <TodoTextInput
          newTodo={true}
          onSave={this.handleSave.bind(this)}
          placeholder="What needs to be done?"
        />
      </header>
    )
  }
}

Header.propTypes = {
  addTodo: PropTypes.func.isRequired
}

export default Header
```

测试如下：

```javascript
import React from 'react'
import { mount } from 'enzyme'
import Header from '../../components/Header'

function setup() {
  const props = {
    addTodo: jest.fn()
  }

  const enzymeWrapper = mount(<Header {...props} />)

  return {
    props,
    enzymeWrapper
  }
}

describe('components', () => {
  describe('Header', () => {
    it('should render self and subcomponents', () => {
      const { enzymeWrapper } = setup()

      expect(enzymeWrapper.find('header').hasClass('header')).toBe(true)

      expect(enzymeWrapper.find('h1').text()).toBe('todos')

      const todoInputProps = enzymeWrapper.find('TodoTextInput').props()
      expect(todoInputProps.newTodo).toBe(true)
      expect(todoInputProps.placeholder).toEqual('What needs to be done?')
    })

    it('should call addTodo if length of text is greater than 0', () => {
      const { enzymeWrapper, props } = setup()
      const input = enzymeWrapper.find('TodoTextInput')
      input.props().onSave('')
      expect(props.addTodo.mock.calls.length).toBe(0)
      input.props().onSave('Use Redux')
      expect(props.addTodo.mock.calls.length).toBe(1)
    })
  })
})
```

### 连接的组件

如果你使用 library 就像 [React Redux ](https://github.com/reactjs/react-redux)，你可能会使用[更高阶组件](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750)喜欢 [`connect() `](https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options)。这使您可以将 Redux 状态注入常规 React 组件。

考虑以下`App`组件：

```javascript
import { connect } from 'react-redux'

class App extends Component { /* ... */ }

export default connect(mapStateToProps)(App)
```

在单元测试中，您通常会`App`像这样导入组件：

```javascript
import App from './App'
```

但是，当您导入它时，实际上是持有由返回的包装器组件`connect()`，而不是`App`组件本身。如果你想测试它与 Redux 的交互，这是一个好消息：你可以[``](https://github.com/reactjs/react-redux/blob/master/docs/api.md#provider-store)用专门为这个单元测试创建的 store 包装它。但有时候你只想测试组件的渲染，没有 Redux 存储。

为了能够在不必处理装饰器的情况下测试App组件，我们建议您也导出未装饰的组件：

```javascript
import { connect } from 'react-redux'

// Use named export for unconnected component (for tests)
export class App extends Component { /* ... */ }

// Use default export for the connected component (for app)
export default connect(mapStateToProps)(App)
```

由于默认导出仍然是装饰组件，所以上图中的导入语句将像以前一样工作，因此您不必更改应用程序代码。但是，您现在可以`App`像这样在测试文件中导入未修饰的组件：

```javascript
// Note the curly braces: grab the named export instead of default export
import { App } from './App'
```

如果你需要两者：

```javascript
import ConnectedApp, { App } from './App'
```

在应用程序本身中，您仍然可以正常导入它：

```javascript
import App from './App'
```

您只能使用命名导出进行测试。

> 关于混合ES6模块和CommonJS的注意事项
>
> 如果您在应用程序源中使用ES6，但是在ES5中编写测试，您应该知道Babel 通过其[交互](http://babeljs.io/docs/usage/modules/#interop)功能处理ES6 `import`和CommonJS`require` 的可互换使用，以便以两种模式运行侧，但行为[稍有不同](https://github.com/babel/babel/issues/2047)。如果在默认导出旁边添加第二个导出，则不能再导入默认`require('./App')`使用。相反，你必须使用`require('./App').default`。

### 中间件

中间件函数`dispatch`在Redux中包装调用行为，因此为了测试这种修改的行为，我们需要模拟`dispatch`调用的行为。

#### 示例

首先，我们需要一个中间件功能。这类似于真正的[redux-thunk](https://github.com/gaearon/redux-thunk/blob/master/src/index.js)。

```javascript
const thunk = ({ dispatch, getState }) => next => action => {
  if (typeof action === 'function') {
    return action(dispatch, getState)
  }

  return next(action)
}
```

我们需要创建一个假的`getState`，`dispatch`和`next`功能。我们用`jest.fn()`来创建存根，但有了其他测试框架，您可能会使用sinon。

invoke函数以与Redux相同的方式运行我们的中间件。

```javascript
const create = () => {
  const store = {
    getState: jest.fn(() => ({})),
    dispatch: jest.fn(),
  };
  const next = jest.fn()

  const invoke = (action) => thunk(store)(next)(action)

  return {store, next, invoke}
};
```

我们测试我们的中间件调用`getState`，`dispatch`以及`next`在正确的时间功能。

```javascript
it(`passes through non-function action`, () => {
  const { next, invoke } = create()
  const action = {type: 'TEST'}
  invoke(action)
  expect(next).toHaveBeenCalledWith(action)
})

it('calls the function', () => {
  const { invoke } = create()
  const fn = jest.fn()
  invoke(fn)
  expect(fn).toHaveBeenCalled()
});

it('passes dispatch and getState', () => {
  const { store, invoke } = create()
  invoke((dispatch, getState) => {
    dispatch('TEST DISPATCH')
    getState();
  })
  expect(store.dispatch).toHaveBeenCalledWith('TEST DISPATCH')
  expect(store.getState).toHaveBeenCalled()
});
```

在某些情况下，你将需要修改`create`使用不同的模拟实现的功能`getState`和`next`。

### 词汇表

- [Enzyme](http://airbnb.io/enzyme/)：Enzyme 是 React 的 JavaScript 测试工具，可以更容易地断言，操纵和遍历 React Components 的输出。

- [React Test Utils](http://facebook.github.io/react/docs/test-utils.html)：测试 React 的实用程序。由 Enzyme 使用。

- [浅层渲染](http://airbnb.io/enzyme/docs/api/shallow.html)：浅层渲染让你实例化一个组件，并有效地获得其`render`方法的结果，而不是递归地将组件递归到DOM。浅层渲染对于单元测试非常有用，您只需测试一个特定的组件，重要的不是它的子项。这也意味着更改子组件不会影响父组件的测试。测试一个组件及其所有的孩子可以用[Enzyme的`mount()`方法](http://airbnb.io/enzyme/docs/api/mount.html)完成，也就是完整的DOM渲染。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18