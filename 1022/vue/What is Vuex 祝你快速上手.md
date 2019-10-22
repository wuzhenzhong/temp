# What is Vuex? 祝你快速上手

### 前言

> 文章涉及的内容可能不全面，但量很多，需要慢慢看。我花了很长的时间整理，用心分享心得，希望对大家有所帮助。但是难免会有打字的错误或理解的错误点，希望发现的可以邮箱告诉我1163675970@qq.com，我会及时的进行修改，只希望对你有所帮助，谢谢。



![img](https://user-gold-cdn.xitu.io/2019/10/21/16dee677624a7e7f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 一、 Vuex 是什么？

> Vuex 是一个专门为 vue.js 应用程序开发的状态（状态就是数据）管理模式，它采用集中式存储管理应用的状态。相当于把组件中的数据提升到一个全局的地方，这个地方就是 Vuex 的 store(仓库)，由 Vuex 统一管理，

> 如果某个组件需要这个数据，直接从 store 中获取。 如果要修改存在 Vuex 中的数据，需要在定义 store 时，定义修改这个数据的方法，这些方法称为 mutation；

- mutation 函数的第一个参数是 state 对象，所有的数据都定义state中，在 mutation 函数中通过 state 可以修改 上面 state 中的数据；
- 注意 mutation 中修改数据只能使用同步的方式，不能再异步的操作中更新数据；如果有异步就需要使用action；
- action 也是更新数据的方式，action 不同于 mutation 的是 action 可以使用异步，但是更新数据仍然需要 commit 对应的 mutation；



![img](https://user-gold-cdn.xitu.io/2019/10/21/16dee67d86e9f067?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





![img](https://user-gold-cdn.xitu.io/2019/10/21/16dee6b9ef1fda63?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 二、使用 Vuex 的步骤

- 在此之前，你需要安装 Vuex，如果是 vue-cli ，在初始化项目时选择 Vuex 把需要放到 Vuex 中的数据存在 state 里面

1. 创建修改这些数据的 mutation
2. 如果这些数据需要异步更新，则创建对应的 action , 并且在 action 中通过 commit mutation 的方式更新数据
3. 最后导出 Vuex 的 stroe 实例，并且为 Vue 的根实例配置 store 属性，配置 store 属性后，在 Vue 实例中可以通过 this.$store 访问 store
4. 在整个 Vue 的应用中，任何地方需要使用该数据，通过 this.$store.state.属性名 的方式获取数据；
5. 如果需要更新这些数据可以通过调用 this.$store.commit(mutation函数名)，如果是异步更新，需要 this.$store.dispatch(action名)

### 三、示例：

```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    // state 就是数据，如果数据定义在 state 中组件中如果要使用这个数据 this.$store.state.属性名 的方式获取
    num: 15
  },
  mutations: {
    // state 中的数据不能被直接修改，如果要修改这些数据，需要使用 mutation，注意 mutation 中不能使用异步更新 state
    add (state) {
      // mutation 函数的第一个参数是 state 对象，所有的数据都定义 state 中，在 mutation 函数中通过 state 可以修改 上面 state 中的数据；
      state.num++
    },
    addNum (state, payload) {
      // mutation 的第二个参数 payload 是在 commit mutation 时传入的参数
      state.num += payload
    }
  },
  actions: {
    // action 可以使用异步，但是更新数据仍然需要 commit 对应的 mutation
    asyncAdd(context) {
      setTimeout(() => {
        context.commit('add')
      }, 1000)
    }
  }
})
复制代码
```

- App.vue

```
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
      <h1>someNum: {{$store.state.num}}</h1>
    </div>
    <router-view/>
  </div>
</template>
<script>
import Bus from './bus'
export default {
  name: 'App',
  data () {
    return {}
  }
}
</script>
复制代码
```

- Home.vue

```
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <div>
      <button @click="add">给上面的sumNum加加</button>
      <button @click="add2">给上面的sumNum加2</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'home',
  methods: {
    add () {
      this.$store.commit('addOne') // commit mutation
      this.$store.dispatch('asyncAdd') // dispatch action
    },
    add2 () {
      this.$store.commit('addNum', 2) // 传递 payload
    }
  }
}
</script>
复制代码
```

### 四、mapState/mapMutations/mapActions

> 如果使用很多个 store 中的数据和方法时通过 this.$store 的方式很不方便，Vuex 提供了三个对应的方法用于批量获取多个 state、mutation、action；

```
<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
// import Bus from '../bus'

import { mapState, mapMutations, mapActions } from 'vuex'

export default {
  name: 'home',
  methods: {
    updateX (e) {
      this.$store.commit('changeX', e.target.value)
    },
    updateSex(e) {
      this.$store.commit('changeSex', e.target.value)
    },
    add () {
       this.addOne() // 等效于 this.$store.commit('addOne')
    },
    add2 () {
       this.addNum(2); // 等效于 this.$store.commit('addNum', 2)
    },
    add3 () {
       this.asyncAdd2(); // 等效于 this.$store.dispatch('asyncAdd')
    },
    ...mapMutations(['addOne', 'addNum']),
    ...mapActions({
      asyncAdd2: 'asyncAdd' // 重命名
    })

  },
  computed: {
    ...mapState(['num'])
  },
  components: {
    HelloWorld
  }
}
</script>
复制代码
```

- 通过 mapState、mapMutations、mapActions 方法结合展开运算符，可以把对应的数据和方法添加到computed 和 methods 中的，这些数据方法都可以通过 this 实例访问

### 五、在项目中使用 vuex

- 个人建议，如果这个数据被多处依赖了最好使用 Vuex，而不是所有的数据都交给 Vuex 托管；
- Vue 书城首页的 轮播图、热门图书使用 Vuex
- store.js

```
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
import * as homeActions from './model/home'
export default new Vuex.Store({
   state: {
      slides: [],
      hotBooks: []
   },
   mutations: {
      fillSides (state, payload) {
         state.slides = payload
      },
      fillHotBooks (state, payload) {
         state.hotBooks = payload
      }
   },
   actions: {
      async getSliders ({ commit }) {
         commit('fillSides', await homeActions.getSliders())
      },
      async getHotBooks ({ commit }) {
         commit('fillHotBooks', await homeActions.getHot())
      }
   }
})
复制代码
```

- Home.vue

```
<template>
  <div >
    <MyHeader>首页</MyHeader>
    <div class="content">
      <swiper :sliders="slides"></swiper>
      <div class="container">
        <h2>热门图书</h2>
        <ul class="container">
          <li v-for="(book, index) in hotBooks" :key="index">
            <img :src="book.bookCover" alt="">
            <b>
              {{book.bookName}}
            </b>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
// @ is an alias to /src
import MyHeader from '@/components/MyHeader.vue'
import swiper from '@/components/Swiper.vue'
// import { getSliders, getHot } from '../model/home'
import { mapState, mapActions } from 'vuex'
export default {
  name: 'home',
  data () {
   return {}
  },
  computed: {
    // slides 和 hotBooks 通过 mapState 从 Vuex store 中获取
    ...mapState(['slides', 'hotBooks'])
  },
  created () {
    // 通过 Vuex 获取图书和轮播信息
    this.getSliders();  // this.$store.dispatch('getSliders')
    this.getHotBooks();  // this.$store.dispatch('getHotBooks')
  },
  methods: {
    ...mapActions(['getSliders', 'getHotBooks'])
  },
  components: {
   MyHeader,
    swiper
  }
}
</script>
<style scoped lang="less">
  .container {
    box-sizing: border-box;
    overflow-x: hidden;
    h2 {
      padding-left: 30px;
    }
    ul li {
      float: left;
      width: 50%;
      margin: 20px 0;
      img {
        display: block;
      }
      b {
        padding-left: 30px;
      }
    }
  }
</style>
```