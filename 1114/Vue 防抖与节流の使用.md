# Vue 防抖与节流の使用

# 场景

在一个电影项目中，我想在电影的列表中，保存下拉的当前位置，防止你切换页面后，再切换回当前的电影列表页，他就又回到电影的第一条数据。
这时候，我不想每次只要滑动一点，就保存当前位置，我想隔一段时间，保存一次，这时候，就可以使用防抖和节流。

# 概念

说白了，**防抖节流就是使用定时器**来实现我们的目的。

**防抖(debounce)：**
在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。

> 典型的案例就是输入框搜索：输入结束后n秒才进行搜索请求，n秒内又输入的内容，则重新计时。

**节流(throttle)：**
规定在一个单位时间内，只能触发一次函数，如果这个单位时间内触发多次函数，只有一次生效。

> 典型的案例就是鼠标不断点击触发，规定在n秒内多次点击只生效一次。

# 用法

## 防抖(debounce)

**下拉列表，隔一段时间保存当前下拉位置。**

我们可以在`mounted`钩子中实现我们的防抖：

```
// 防抖  定时器
let timer;

//list就是电影列表	ref="list"	$el获取DOM元素
this.$refs.list.$el.addEventListener("scroll", e => {
	console.log("---->",e.target.scrollTop)	//不使用防抖
	if (timer) {
		//清空timer
		clearTimeout(timer);
	}
	timer = setTimeout(() => {
		console.log(e.target.scrollTop)	//使用防抖
		
		//在sessionStorage中保存当前下拉位置
		// sessionStorage.setItem("position", e.target.scrollTop);
	}, 75);		//75mm为最佳
});
复制代码
```

**效果演示**(隔一段时间保存当前位置)：
加`---->`为不使用防抖，没加的则使用防抖



![图片加载失败!](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>)



**输入框搜索隔段时间进行搜索请求：**

```
<template>
  <div>
    <input type="text" @keyup="debounce" />
  </div>
</template>

<script>
//定义 timer
let timer;
export default {
  methods: {
    debounce: function() {
      if (timer) {
        clearTimeout(timer);
      }
      timer = setTimeout(() => {
        console.log("防抖...");
        timer = undefined;
      }, 2000);
    }
  }
};
</script>
复制代码
```



![图片加载失败!](https://gitee.com/gitee_fanjunyang/JueJin/raw/master/images/Vue%E9%98%B2%E6%8A%96%E8%8A%82%E6%B5%81_2.gif)



## 节流(throttle)

**在n秒内点击多次，只有一次生效。**

```
<template>
  <div>
    <button @click="throttle">按钮</button>
  </div>
</template>

<script>
//定义
let timer, lastTime;
export default {
  methods: {
    throttle: function() {
      let now = +new Date();
      if (lastTime && lastTime - now < 200) { //在200ms内点击多次，只有一次生效
        clearTimeout(timer);
      }
      timer = setTimeout(() => {
        console.log("点击...");
        lastTime = +new Date();
      }, 200);
    }
  }
};
</script>
复制代码
```

效果演示：



![图片加载失败!](https://gitee.com/gitee_fanjunyang/JueJin/raw/master/images/Vue%E9%98%B2%E6%8A%96%E8%8A%82%E6%B5%81_3.gif)



# 补充

当然，也可以对这两个方法进行封装，以便在多处使用。

```
/**
 * 函数防抖 (只执行最后一次点击)
 */
export const Debounce = (fn, t) => {
    let delay = t || 500;
    let timer;
    return function () {
        let args = arguments;
        if(timer){
            clearTimeout(timer);
        }
        timer = setTimeout(() => {
            timer = null;
            fn.apply(this, args);
        }, delay);
    }
};
/*
 * 函数节流
 */
export const Throttle = (fn, t) => {
    let last;
    let timer;
    let interval = t || 500;
    return function () {
        let args = arguments;
        let now = +new Date();
        if (last && now - last < interval) {
            clearTimeout(timer);
            timer = setTimeout(() => {
                last = now;
                fn.apply(this, args);
            }, interval);
        } else {
            last = now;
            fn.apply(this, args);
        }
    }
};
```