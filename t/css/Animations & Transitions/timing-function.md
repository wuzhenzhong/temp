# 定时功能 | <timing-function>

- 贡献者5人

  

`<single-transition-timing-function>` [CSS](https://developer.mozilla.org/zh-CN/CSS) 数据类型表示一个数学函数，它描述了在一个过渡或动画中一维数值的改变速度。这实质上让你可以自己定义一个加速度曲线，以便动画的速度在动画的过程中可以进行改变, 这些函数通常被称为缓动函数。

这是一个表示时间输出比率的函数，表示为[``](https://developer.mozilla.org/zh-CN/docs/Web/CSS/number)，`0, 0`代表初始状态，`1, 1` 代表终止状态。



![img](https://ask.qcloudimg.com/http-save/devdocs/eroiwc9zxz.png)

![img](https://ask.qcloudimg.com/http-save/devdocs/b8ih4v7z6i.png)

输出比可以大于1.0（或者小于0.0）。这是因为在一种反弹效果中，动画是可以比最后的状态走的更远的，然后再回到最终状态。



尽管如此，如果输出值超出其可能的范围，例如颜色大于`255`或小于的分量`0`，则将该值限制为其最接近的允许值（在颜色分量`255`和`0`分别的情况下）。一些`cubic-bezier()`曲线显示这个属性。

## 值



CSS 支持两种时间函数：立方贝塞尔曲线（cubic Bézier curves）的子集和阶梯函数。最有用的函数是易于使用的关键字。



### `cubic-bezier()` 时间函数



![img](https://ask.qcloudimg.com/http-save/devdocs/psh403xd1e.png)

`cubic-bezier()` 定义了一条 [立方贝塞尔曲线（cubic Bézier curve）](http://en.wikipedia.org/wiki/Bézier_curve#Cubic_B.C3.A9zier_curves)。这些曲线是连续的，一般用于动画的平滑变换，也被称为缓动函数（*easing functions*）。



一条立方贝塞尔曲线需要四个点来定义，P0 、P1 、P2 和 P3，P0 和 P3 是起点和终点，这两个点被作为比例固定在坐标系上，横轴为时间比例，纵轴为完成状态。P0 是 `(0, 0)，表示初始时间和初始状态。`P3 是 `(1, 1)` ，表示终止时间和终止状态。



不是所有的三次Bézier曲线都适合作为定时函数，因为并不是所有的都是[数学函数](https://en.wikipedia.org/wiki/Function_%28mathematics%29)。即对于给定的横坐标具有零个或一个值的曲线。如果P0和P3固定为CSS定义，则三次Bézier曲线是一个函数，因此当且仅当P1和P2的横坐标均在该`[0, 1]`范围内时才是有效的。

P1或P2纵坐标的三次贝塞尔曲线`[0, 1]`可能会产生*弹跳*效应。

当你指定一个无效的`cubic-bezier`曲线时，CSS忽略整个属性。

**语法**

```javascript
cubic-bezier(x1, y1, x2, y2)
```

哪里：

***x1*****，** ***y1*****，** ***x2*****，** _ **y2** _`<number>`表示横坐标，P1和P2点的纵坐标定义三次Bézier曲线。x1和x2必须在0,1范围内，否则该值无效。

#### 示例



`CSS`中允许使用这些贝塞尔曲线：



```javascript
/* The canonical Bézier curve with four <number> in the [0,1] range. */
cubic-bezier(0.1, 0.7, 1.0, 0.1)   

/* Using <integer> is valid as any <integer> is also a <number>. */
cubic-bezier(0, 0, 1, 1)           

/* Negative values for ordinates are valid, leading to bouncing effects.*/
cubic-bezier(0.1, -0.6, 0.2, 0)    

/* Values > 1.0 for ordinates are also valid. */
cubic-bezier(0, 1.1, 0.8, 4)
```

这些三次贝塞尔曲线的定义是无效的：

```javascript
/* Though the animated output type may be a color, 
   Bézier curves work w/ numerical ratios.*/
cubic-bezier(0.1, red, 1.0, green) 

/* Abscissas must be in the [0, 1] range or 
   the curve is not a function of time. */
cubic-bezier(2.45, 0.6, 4, 0.1)    

/* The two points must be defined, there is no default value. */
cubic-bezier(0.3, 2.1)   

/* Abscissas must be in the [0, 1] range or 
   the curve is not a function of time. */
cubic-bezier(-1.9, 0.3, -0.2, 2.1) 
```

### `steps()`定时功能的类

`steps()`函数表示法定义了一个[阶跃函数](https://en.wikipedia.org/wiki/Step_function)除以输出值的域在等距离的步骤。

这个阶梯函数的子类有时也被称为阶梯函数。

![img](https://ask.qcloudimg.com/http-save/devdocs/jx2ak9qy21.png)

```
steps(2, start)
```

![img](https://ask.qcloudimg.com/http-save/devdocs/f7l81ujnao.png)

```
steps(4, end)
```

#### 语法

```javascript
steps(number_of_steps, direction)
```

哪里：

number_of_steps是严格正数`<integer>`，表示构成步进函数的等距距离的数量.direction是指示函数是[左连续的还是右连续](https://en.wikipedia.org/wiki/Left-continuous#Directional_and_semi-continuity)的关键字：

- `start` 表示一个左连续函数，这样动画开始时就会发生第一步;

- `end` 表示一个右连续函数，以便动画结束时最后一步的发生。

`end` 是默认的。

#### 示例

这些定时功能是有效的：

```javascript
/* There is 5 treads, the last one happens 
   right before the end of the animation. */
steps(5, end)

/* A two-step staircase, the first one happening 
   at the start of the animation. */
steps(2, start)        

/* The second parameter is optional. */
steps(2)               
```

这些定时功能无效：

```javascript
/* The first parameter must be an <integer> and 
   cannot be a real value, even if it is equal to one. */
steps(2.0, end)

/* The amount of steps must be non-negative. */
steps(-3, start)

/* There must be at least one step.*/
steps(0, end)       
```

### 定时功能的`frames()`类

注意：frame（）计时功能的名称目前正在讨论中，因此在确定之前，它目前在浏览器发布版本中是不可用的。



frames（）函数符号定义了将输出值的域划分为等距间隔的帧函数。 frames（）和steps（）之间的区别在于frame（），start（0％）和end（100％）状态时间与其他间隔的时间是相等的。

![img](data:image/svg+xml;%20qs=0.85;base64,PCEtLSB2aW06IHNldCBleHBhbmR0YWIgdHM9MiBzdz0yIHR3PTgwOiAtLT4NCjxzdmcgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIg0KICB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayINCiAgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgdmlld0JveD0iMCAwIDMxMCAxNjAiPg0KICA8ZGVmcz4NCiAgICA8c3R5bGUgdHlwZT0idGV4dC9jc3MiPg0KICAgIHN2ZyB7DQogICAgICBmb250LXNpemU6IDE1cHg7DQogICAgICBmb250LWZhbWlseTogc2Fucy1zZXJpZjsNCiAgICB9DQoNCiAgICAvKg0KICAgICAqIExpbmUgd29yaw0KICAgICAqLw0KICAgIC5heGlzLCAudGlja01hcmsgew0KICAgICAgc3Ryb2tlOiBibGFjazsNCiAgICAgIHN0cm9rZS13aWR0aDogMS4yOw0KICAgICAgZmlsbDogbm9uZTsNCiAgICAgIHN0cm9rZS1saW5lY2FwOiBzcXVhcmU7DQogICAgfQ0KICAgIC5jdXJ2ZSB7DQogICAgICBmaWxsOiBub25lOw0KICAgICAgc3Ryb2tlOiBibHVlOw0KICAgICAgc3Ryb2tlLXdpZHRoOiAzOw0KICAgICAgc3Ryb2tlLWxpbmVjYXA6IHJvdW5kOw0KICAgIH0NCiAgICAuZG90dGVkIHsNCiAgICAgIGZpbGw6IG5vbmU7DQogICAgICBzdHJva2U6IGJsdWU7DQogICAgICBzdHJva2Utd2lkdGg6IDEuNTsNCiAgICAgIHN0cm9rZS1saW5lY2FwOiByb3VuZDsNCiAgICAgIHN0cm9rZS1kYXNoYXJyYXk6IDIgNjsNCiAgICB9DQogICAgLmZpbGxlZEVuZFBvaW50IHsNCiAgICAgIGZpbGw6IGJsdWU7DQogICAgICBzdHJva2U6IG5vbmU7DQogICAgfQ0KICAgIC5vcGVuRW5kUG9pbnQgew0KICAgICAgZmlsbDogbm9uZTsNCiAgICAgIHN0cm9rZTogYmx1ZTsNCiAgICAgIHN0cm9rZS13aWR0aDogMS41Ow0KICAgIH0NCg0KICAgIC8qDQogICAgICogQmFja2dyb3VuZA0KICAgICAqLw0KICAgIC5ncmFwaEJhY2tncm91bmQgew0KICAgICAgc3Ryb2tlOiBub25lOw0KICAgICAgZmlsbDogd2hpdGU7DQogICAgfQ0KDQogICAgLyoNCiAgICAgKiBNYXNraW5nDQogICAgICovDQogICAgLmxpbmVLbm9ja291dCB7DQogICAgICBmaWxsOiBub25lOw0KICAgICAgc3Ryb2tlOiAjNTU1Ow0KICAgICAgc3Ryb2tlLXdpZHRoOiAyOw0KICAgIH0NCiAgICAuY2lyY2xlS25vY2tvdXQgew0KICAgICAgZmlsbDogIzU1NTsNCiAgICAgIHN0cm9rZTogbm9uZTsNCiAgICB9DQoNCiAgICAvKg0KICAgICAqIFRleHQgbGFiZWxzDQogICAgICovDQogICAgLmF4aXNWYWx1ZSB7DQogICAgICBmaWxsOiBncmV5Ow0KICAgIH0NCiAgICAuYXhpc1ZhbHVlLnZlcnQgew0KICAgICAgdGV4dC1hbmNob3I6IGVuZDsNCiAgICB9DQogICAgLmF4aXNWYWx1ZS5ob3J6IHsNCiAgICAgIHRleHQtYW5jaG9yOiBtaWRkbGU7DQogICAgfQ0KICAgIC5wb2ludExhYmVsIHsNCiAgICAgIGZpbGw6IGJsYWNrOw0KICAgICAgZm9udC1zaXplOiAxMXB4Ow0KICAgIH0NCiAgICAucGFyYW1MYWJlbCB7DQogICAgICBmb250LXNpemU6IDE1cHg7DQogICAgICB0ZXh0LWFuY2hvcjogbWlkZGxlOw0KICAgIH0NCiAgICA8L3N0eWxlPg0KICAgIDwhLS0gR3JhcGggYmFja2dyb3VuZCAtLT4NCiAgICA8ZyBpZD0iZ3JhcGgiIHN0eWxlPSJmb250LXNpemU6IDEwcHgiPg0KICAgICAgPCEtLSBCYWNraW5nIC0tPg0KICAgICAgPHJlY3QgeT0iLTEwMCIgd2lkdGg9IjEwMCIgaGVpZ2h0PSIxMDAiIGNsYXNzPSJncmFwaEJhY2tncm91bmQiLz4NCiAgICAgIDwhLS0gWSBheGlzIC0tPg0KICAgICAgPGxpbmUgeTI9Ii0xMDAiIGNsYXNzPSJheGlzIi8+DQogICAgICA8bGluZSB5MT0iLTUwIiB4Mj0iLTUiIHkyPSItNTAiIGNsYXNzPSJ0aWNrTWFyayIvPg0KICAgICAgPGxpbmUgeTE9Ii0xMDAiIHgyPSItNSIgeTI9Ii0xMDAiIGNsYXNzPSJ0aWNrTWFyayIvPg0KICAgICAgPHRleHQgeD0iLTkiIHk9Ii01MCIgZHk9IjAuM2VtIiBjbGFzcz0iYXhpc1ZhbHVlIHZlcnQiPjAuNTwvdGV4dD4NCiAgICAgIDx0ZXh0IHg9Ii05IiB5PSItMTAwIiBkeT0iMC4zZW0iIGNsYXNzPSJheGlzVmFsdWUgdmVydCI+MTwvdGV4dD4NCiAgICAgIDwhLS0gWCBheGlzIC0tPg0KICAgICAgPGxpbmUgeDI9IjEwMCIgY2xhc3M9ImF4aXMiLz4NCiAgICAgIDxsaW5lIHgxPSI1MCIgeDI9IjUwIiB5Mj0iNSIgY2xhc3M9InRpY2tNYXJrIi8+DQogICAgICA8bGluZSB4MT0iMTAwIiB4Mj0iMTAwIiB5Mj0iNSIgY2xhc3M9InRpY2tNYXJrIi8+DQogICAgICA8dGV4dCB4PSI1MCIgeT0iNy41IiBkeT0iMS4xZW0iIGNsYXNzPSJheGlzVmFsdWUgaG9yeiI+MC41PC90ZXh0Pg0KICAgICAgPHRleHQgeD0iMTAwIiB5PSI3LjUiIGR5PSIxLjFlbSIgY2xhc3M9ImF4aXNWYWx1ZSBob3J6Ij4xPC90ZXh0Pg0KICAgICAgPCEtLSBPcmlnaW4gLS0+DQogICAgICA8dGV4dCB4PSItMC41ZW0iIHk9IjFlbSIgY2xhc3M9ImF4aXNWYWx1ZSIgdGV4dC1hbmNob3I9ImVuZCI+MDwvdGV4dD4NCiAgICA8L2c+DQogIDwvZGVmcz4NCiAgPGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMzAgMTEwKSI+DQogICAgPCEtLSBmcmFtZXMoMikgLS0+DQogICAgPGc+DQogICAgICA8ZGVmcz4NCiAgICAgICAgPG1hc2sgaWQ9ImZyYW1lczJHcmlkTWFzayIgbWFza1VuaXRzPSJ1c2VyU3BhY2VPblVzZSINCiAgICAgICAgICB4PSItMzAiIHk9Ii0xMTAiIHdpZHRoPSIxMzUiIGhlaWdodD0iMTM1Ij4NCiAgICAgICAgICA8cmVjdCB4PSItMzAiIHk9Ii0xMTAiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIGZpbGw9IndoaXRlIi8+DQogICAgICAgICAgPGNpcmNsZSBjeD0iNTAiIHI9IjMiIGNsYXNzPSJjaXJjbGVLbm9ja291dCIvPg0KICAgICAgICA8L21hc2s+DQogICAgICAgIDxtYXNrIGlkPSJmcmFtZXMyTGluZU1hc2siPg0KICAgICAgICAgIDxyZWN0IHg9Ii01IiB5PSItMTA1IiB3aWR0aD0iMTEwIiBoZWlnaHQ9IjExMCIgZmlsbD0id2hpdGUiLz4NCiAgICAgICAgICA8Y2lyY2xlIGN4PSI1MCIgcj0iMyIgZmlsbD0iYmxhY2siLz4NCiAgICAgICAgICA8Y2lyY2xlIGN4PSIxMDAiIGN5PSItMTAwIiByPSIzIiBmaWxsPSJibGFjayIvPg0KICAgICAgICA8L21hc2s+DQogICAgICA8L2RlZnM+DQogICAgICA8dXNlIHhsaW5rOmhyZWY9IiNncmFwaCIgbWFzaz0idXJsKCNmcmFtZXMyR3JpZE1hc2spIi8+DQogICAgICA8ZyBtYXNrPSJ1cmwoI2ZyYW1lczJMaW5lTWFzaykiPg0KICAgICAgICA8cGF0aCBkPSJNMCAwaDUwbTAtMTAwaDUwIiBjbGFzcz0iY3VydmUiLz4NCiAgICAgICAgPHBhdGggZD0iTTUwIDB2LTEwMCIgY2xhc3M9ImRvdHRlZCIvPg0KICAgICAgPC9nPg0KICAgICAgPGNpcmNsZSByPSIzLjUiIGNsYXNzPSJmaWxsZWRFbmRQb2ludCIvPg0KICAgICAgPGNpcmNsZSBjeD0iNTAiIHI9IjMiIGNsYXNzPSJvcGVuRW5kUG9pbnQiLz4NCiAgICAgIDxjaXJjbGUgY3g9IjUwIiBjeT0iLTEwMCIgcj0iMy41IiBjbGFzcz0iZmlsbGVkRW5kUG9pbnQiLz4NCiAgICAgIDxjaXJjbGUgY3g9IjEwMCIgY3k9Ii0xMDAiIHI9IjMuNSIgY2xhc3M9ImZpbGxlZEVuZFBvaW50Ii8+DQogICAgICA8dGV4dCB4PSI1MCIgeT0iNDAiIGNsYXNzPSJwYXJhbUxhYmVsIj5mcmFtZXMoMik8L3RleHQ+DQogICAgPC9nPg0KICAgIDwhLS0gZnJhbWVzKDQpIC0tPg0KICAgIDxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKDE3MCAwKSI+DQogICAgICA8ZGVmcz4NCiAgICAgICAgPG1hc2sgaWQ9ImZyYW1lczRHcmlkTWFzayIgbWFza1VuaXRzPSJ1c2VyU3BhY2VPblVzZSINCiAgICAgICAgICB4PSItMzAiIHk9Ii0xMTAiIHdpZHRoPSIxMzUiIGhlaWdodD0iMTM1Ij4NCiAgICAgICAgICA8cmVjdCB4PSItMzAiIHk9Ii0xMTAiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIGZpbGw9IndoaXRlIi8+DQogICAgICAgICAgPGNpcmNsZSBjeD0iMjUiIHI9IjMiIGNsYXNzPSJjaXJjbGVLbm9ja291dCIvPg0KICAgICAgICA8L21hc2s+DQogICAgICAgIDxtYXNrIGlkPSJmcmFtZXM0TGluZU1hc2siPg0KICAgICAgICAgIDxyZWN0IHg9Ii01IiB5PSItMTA1IiB3aWR0aD0iMTEwIiBoZWlnaHQ9IjExMCIgZmlsbD0id2hpdGUiLz4NCiAgICAgICAgICA8Y2lyY2xlIGN4PSIyNSIgcj0iMyIgZmlsbD0iYmxhY2siLz4NCiAgICAgICAgICA8Y2lyY2xlIGN4PSI1MCIgY3k9Ii0zMy4zIiByPSIzIiBmaWxsPSJibGFjayIvPg0KICAgICAgICAgIDxjaXJjbGUgY3g9Ijc1IiBjeT0iLTY2LjYiIHI9IjMiIGZpbGw9ImJsYWNrIi8+DQogICAgICAgICAgPGNpcmNsZSBjeD0iMTAwIiBjeT0iLTEwMCIgcj0iMyIgZmlsbD0iYmxhY2siLz4NCiAgICAgICAgPC9tYXNrPg0KICAgICAgPC9kZWZzPg0KICAgICAgPHVzZSB4bGluazpocmVmPSIjZ3JhcGgiIG1hc2s9InVybCgjZnJhbWVzNEdyaWRNYXNrKSIvPg0KICAgICAgPGcgbWFzaz0idXJsKCNmcmFtZXM0TGluZU1hc2spIj4NCiAgICAgICAgPHBhdGggZD0iTTAgMGgyNW0wLTMzLjNoMjVtMC0zMy4zaDI1bTAtMzMuM2gyNSIgY2xhc3M9ImN1cnZlIi8+DQogICAgICAgIDxwYXRoIGQ9Ik0yNSAwdi0zMy4zbTI1IDB2LTMzLjNtMjUgMHYtMzMuMyIgY2xhc3M9ImRvdHRlZCIvPg0KICAgICAgPC9nPg0KICAgICAgPGNpcmNsZSByPSIzLjUiIGNsYXNzPSJmaWxsZWRFbmRQb2ludCIvPg0KICAgICAgPGNpcmNsZSBjeD0iMjUiIHI9IjMiIGNsYXNzPSJvcGVuRW5kUG9pbnQiLz4NCiAgICAgIDxjaXJjbGUgY3g9IjI1IiBjeT0iLTMzLjMiIHI9IjMuNSIgY2xhc3M9ImZpbGxlZEVuZFBvaW50Ii8+DQogICAgICA8Y2lyY2xlIGN4PSI1MCIgY3k9Ii0zMy4zIiByPSIzIiBjbGFzcz0ib3BlbkVuZFBvaW50Ii8+DQogICAgICA8Y2lyY2xlIGN4PSI1MCIgY3k9Ii02Ni42IiByPSIzLjUiIGNsYXNzPSJmaWxsZWRFbmRQb2ludCIvPg0KICAgICAgPGNpcmNsZSBjeD0iNzUiIGN5PSItNjYuNiIgcj0iMyIgY2xhc3M9Im9wZW5FbmRQb2ludCIvPg0KICAgICAgPGNpcmNsZSBjeD0iNzUiIGN5PSItMTAwIiByPSIzLjUiIGNsYXNzPSJmaWxsZWRFbmRQb2ludCIvPg0KICAgICAgPGNpcmNsZSBjeD0iMTAwIiBjeT0iLTEwMCIgcj0iMy41IiBjbGFzcz0iZmlsbGVkRW5kUG9pbnQiLz4NCiAgICAgIDx0ZXh0IHg9IjUwIiB5PSI0MCIgY2xhc3M9InBhcmFtTGFiZWwiPmZyYW1lcyg0KTwvdGV4dD4NCiAgICA8L2c+DQogIDwvZz4NCjwvc3ZnPg0K)

```
frames(2), frames(4)
```

#### 语法

```javascript
frames(number_of_frames)
```

哪里:

number_of_frames是严格正数`<integer>`，表示构成步进函数的等距间隔的数量。

#### 示例

这些定时功能是有效的：

```javascript
/* The parameter is a positive integer. */
frames(10)               
```

**注意**：您可以在我们的GitHub中[使用frames（）函数](https://mdn.github.io/css-examples/animation-frames-timing-function/index-transitions.html)查看[工作过渡示例](https://mdn.github.io/css-examples/animation-frames-timing-function/index-transitions.html)。

这些定时功能无效：

```javascript
/* The parameter must be an <integer> and 
   cannot be a real value, even if it is equal to one. */
frames(2.0)

/* The amount of frames must be non-negative. */
frames(-3)

/* There must be at least two frames.*/
frames(0)       
```

### 常见计时功能的关键词

#### `linear`

![img](https://ask.qcloudimg.com/http-save/devdocs/3p8kshdiv4.png)

这个关键字代表了计时功能`cubic-bezier(0.0, 0.0, 1.0, 1.0)`。使用这个定时功能，动画以恒定的速度从其初始状态到最终状态。

#### `ease`

![img](https://ask.qcloudimg.com/http-save/devdocs/eu59b1nf1h.png)

这个关键字代表了计时功能`cubic-bezier(0.25, 0.1, 0.25, 1.0)`。这个功能类似于`ease-in-out`，虽然开始时加速比较急剧，但在时间到了一半之前开始减速，并且慢慢减缓。

#### `ease-in`

![img](https://ask.qcloudimg.com/http-save/devdocs/yet0l10j72.png)

这个关键字代表了计时功能`cubic-bezier(0.42, 0.0, 1.0, 1.0)`。动画开始缓慢，然后逐渐加速，直到达到最终状态，动画突然停止。

#### `ease-in-out`

![img](https://ask.qcloudimg.com/http-save/devdocs/pn40qvxn48.png)

这个关键字代表了计时功能`cubic-bezier(0.42, 0.0, 0.58, 1.0)`。使用此计时功能，动画开始缓慢，加速更快，然后在接近其最终状态时变慢。在开始时，它与`ease-in`功能类似; 最后时，与`ease-out`功能类似。

#### `ease-out`

![img](https://ask.qcloudimg.com/http-save/devdocs/xfj1h4u63h.png)

这个关键字代表了计时功能`cubic-bezier(0.0, 0.0, 0.58, 1.0)`。动画开始迅速，然后在接近最终状态时放慢速度。

#### `step-start`

![img](https://ask.qcloudimg.com/http-save/devdocs/t0yln0jbng.png)

这个关键字代表了计时功能`steps(1, start)`。使用这个定时功能，动画立即跳转到结束状态并保持在该位置直到动画结束。

#### `step-end`

![img](https://ask.qcloudimg.com/http-save/devdocs/104mwr819g.png)

这个关键字代表了计时功能`steps(1, end)`。使用这个定时功能，动画保持其初始状态直到结束，直接跳到最终位置。

## 规范

| Specification                                                | Status        | Comment                                                      |
| :----------------------------------------------------------- | :------------ | :----------------------------------------------------------- |
| CSS AnimationsThe definition of '<single-transition-timing-function>' in that specification. | Working Draft | Defines <single-timing-function> as synonym for <single-transition-timing-function> of CSS Transitions Module. |
| CSS TransitionsThe definition of '<single-transition-timing-function>' in that specification. | Working Draft | Initial definition                                           |

## 浏览器兼容性

| Feature                            | Firefox (Gecko) | Chrome      | Internet Explorer | Opera       | Safari        |
| :--------------------------------- | :-------------- | :---------- | :---------------- | :---------- | :------------ |
| Basic support                      | 4.0 (2.0)       | 4.0         | 10.0              | 10.5        | 3.1           |
| cubic-bezier() with ordinate ∉ 0,1 | 4.0 (2.0)       | 16.0        | 10.0              | 12.1        | Nightly build |
| steps()                            | 4.0 (2.0)       | 8.0         | 10.0              | 12.1        | 5.1           |
| frames()                           | No support1     | No support1 | No support        | No support1 | ?             |

| Feature                            | Firefox Mobile (Gecko) | Android | IE Phone   | Opera Mobile | Safari Mobile |
| :--------------------------------- | :--------------------- | :------ | :--------- | :----------- | :------------ |
| Basic support                      | 4.0 (2.0)              | 4.0     | No support | 10.0         | 2.0           |
| cubic-bezier() with ordinate ∉ 0,1 | 4.0 (2.0)              | (Yes)   | No support | ?            | ?             |
| steps()                            | 4.0 (2.0)              | 4.0     | No support | ?            | 5.0           |
| frames()                           | No support1            | ?       | No support | No support   | ?             |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-06-28