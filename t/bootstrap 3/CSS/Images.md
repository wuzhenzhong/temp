# 图片 | Images

## Images

### 响应式图像

[纠错](javascript:;)

Bootstrap 3中的图像可以通过添加`.img-responsive`类来响应友好。这适用`max-width: 100%;`，`height: auto;`并`display: block;`在图像，使其很好地扩展到父元素。

要使用`.img-responsive`该类的图像居中，请使用`.center-block`而不是`.text-center`。有关`.center-block`使用的更多详细信息，请参阅帮助程序类部分。

##### SVG图像和IE 8-10

在Internet Explorer 8-10中，SVG图像的`.img-responsive`大小不成比例。要解决此问题，请`width: 100% \9;`在必要时添加。Bootstrap不会自动应用，因为它会导致其他图像格式的复杂化。

```javascript
<img src="..." class="img-responsive" alt="Responsive image">
```

### 图像形状

将`<img>`元素添加到元素中以轻松设置任何项目中的图像样式。

##### 跨浏览器兼容性

请记住，Internet Explorer 8不支持圆角。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-images-ie-rounded-corners)

```javascript
<img src="..." alt="..." class="img-rounded">
<img src="..." alt="..." class="img-circle">
<img src="..." alt="..." class="img-thumbnail">
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com