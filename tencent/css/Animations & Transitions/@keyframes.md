# 关键帧 | @keyframes

- 贡献者1人

  

在CSS中，@keyframes根据你定义的样式规则来更有效的控制动画队列中的每一个中间步骤（或者每一个路径点）。用@keyframes来制定动画规则会比使用transition(过渡)来获得更细腻更全面的动画效果。

```javascript
@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}
```

JavaScript可以通过CSS对象模型提供的[`CSSKeyframesRule`](https://developer.mozilla.org/en-US/docs/Web/API/CSSKeyframesRule)接口来访问@keyframes







要使用关键帧动画,需要先创建一个具名的`@keyframes`规则，以便后续使用[`animation-name`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-name)属性来调用指定的`@keyframes`. 每个`@keyframes`规则包含多个关键帧，也就是一段样式块语句，每个关键帧有一个百分比值作为名称，代表在动画进行中，在哪个阶段触发这个帧所包含的样式。



你可以在每一个百分比关键帧内写上任意的规则，它们会在该触发的时候被触发。



### 让关键帧序列生效



为了让一个关键帧列表有效，它必须至少包含了对0%（或from）和100%（或to）即动画的开头帧和结束帧的定义。 如果都没有进行定义，那么整个@keyframes 是无效的，不能使用。



如果在关键帧的样式中使用了不能用作动画的属性，那么这些属性会被忽略掉，支持动画的属性仍然是有效的，不受波及。



### 重复定义



如果多个关键帧使用同一个名称，以最后一次定义的为准。 `@keyframes`不存在层叠样式(cascade)的情况，所以动画在一个时刻（阶段）只会使用一个的关键帧的数据。



如果一个@keyframes 里的关键帧的百分比存在重复的情况，以最后一次定义的关键帧为准。 因为`@keyframes`的规则不存在层叠样式(cascade)的情况，即使多个关键帧设置相同的百分值也不会全部执行。



关键帧中属性的遗漏



如果一个关键帧中没有出现其他关键帧中的属性，那么这个属性将使用插值(不能使用插值的属性除外, 这些属性会被忽略掉)。例如：



```javascript
@keyframes identifier {
  0% { top: 0; left: 0; }
  30% { top: 50px; }
  68%, 72% { left: 50px; }
  100% { top: 100px; left: 100%; }
}
```

例子中，"top"属性分别出现在 `0%`,`30%`和`100%`的关键帧中，"left"属性分别出现在`0%`,`68%`和`100%` 关键帧中.



### 重复定义



如果某一个关键帧出现了重复的定义，且重复的关键帧中的css属性值不同，以最后一次定义的属性为准。例如：



```javascript
@keyframes identifier {
  0% { top: 0; }
  50% { top: 30px; left: 20px; }
  50% { top: 10px; }
  100% { top: 0; }
}
```

上面这个例子中，50% 关键帧中设置的属性`top: 10px是有效的，但是其他的属性(left: 20px)会被忽略`



 层叠样式(cascade) 的特性从Firefox 14版本开始可以使用了。 拿上面的例子来说，对于 `50%` 关键帧，`left: 20px` 这个值不会被忽略掉。 目前这种特性还没写入规范，但是已经在探讨中了。



### 关键帧中的 !important 关键词



如果某一个关键帧出现了重复的定义，且重复的关键帧中的css属性值不同，以最后一次定义的属性为准。例如：



```javascript
@keyframes important1 {
  from { margin-top: 50px; }
  50%  { margin-top: 150px !important; } /* ignored */
  to   { margin-top: 100px; }
}

@keyframes important2 {
  from { margin-top: 50px;
         margin-bottom: 100px; }
  to   { margin-top: 150px !important; /* ignored */
         margin-bottom: 50px; }
}
```

## 语法

### 值

`<custom-ident>`标识关键帧列表的名称。这必须与CSS语法中的标识符生成相匹配。`from`起始偏移量为`0%`。`to`一个结束偏移量`100%`。`<percentage>`指定关键帧出现的动画序列的时间百分比。

### 形式语法

```javascript
@keyframes <keyframes-name> {
  <keyframe-block-list>
}where 
<keyframes-name> = <custom-ident> | <string>
<keyframe-block-list> = <keyframe-block>+
where 
<keyframe-block> = <keyframe-selector># {
  <declaration-list>
}
where 
<keyframe-selector> = from | to | <percentage>
```

## 实例

请参阅使用CSS动画作为示例。

## 规范

| Specification                                                | Status        | Comment |
| :----------------------------------------------------------- | :------------ | :------ |
| CSS AnimationsThe definition of '@keyframes' in that specification. | Working Draft |         |

## 浏览器兼容性

| Feature                        | Chrome            | Edge  | Firefox (Gecko)           | Internet Explorer | Opera         | Safari (WebKit)  |
| :----------------------------- | :---------------- | :---- | :------------------------ | :---------------- | :------------ | :--------------- |
| Basic support                  | (Yes)-webkit 43.0 | (Yes) | 5.0 (5.0)-moz 16.0 (16.0) | 10                | 12 -o 12.10 # | 4.0-webkit 9.0 # |
| ignore !important declarations | ?                 | ?     | 19 (19)                   | ?                 | ?             | ?                |

| Feature                        | Android      | Edge  | Firefox Mobile (Gecko)    | IE Phone | Opera Mobile | Safari Mobile |
| :----------------------------- | :----------- | :---- | :------------------------ | :------- | :----------- | :------------ |
| Basic support                  | (Yes)-webkit | (Yes) | 5.0 (5.0)-moz 16.0 (16.0) | ?        | ?            | ?             |
| ignore !important declarations | ?            | ?     | 19.0 (19)                 | ?        | ?            | ?             |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com