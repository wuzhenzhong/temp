# ColorPicker 颜色选择器

## ColorPicker 颜色选择器

用于颜色选择，支持多种格式。

### 基础用法

- 有默认值
- 无默认值

使用 v-model 与 Vue 实例中的一个变量进行双向绑定，绑定的变量需要是字符串类型。

```javascript
<div class="block">
  <span class="demonstration">有默认值</span>
  <el-color-picker v-model="color1"></el-color-picker>
</div>
<div class="block">
  <span class="demonstration">无默认值</span>
  <el-color-picker v-model="color2"></el-color-picker>
</div>

<script>
  export default {
    data() {
      return {
        color1: '#409EFF',
        color2: null
      }
    }
  };
</script>
```

### 选择透明度

ColorPicker 支持普通颜色，也支持带 Alpha 通道的颜色，通过`show-alpha`属性即可控制是否支持透明度的选择。

```javascript
<el-color-picker v-model="color3" show-alpha></el-color-picker>

<script>
  export default {
    data() {
      return {
        color3: 'rgba(19, 206, 102, 0.8)'
      }
    }
  };
</script>
```

### 预定义颜色

ColorPicker 支持预定义颜色

```javascript
<el-color-picker
  v-model="color5"
  show-alpha
  :predefine="predefineColors">
</el-color-picker>

<script>
  export default {
    data() {
      return {
        color5: 'rgba(255, 69, 0, 0.68)',
        predefineColors: [
          '#ff4500',
          '#ff8c00',
          '#ffd700',
          '#90ee90',
          '#00ced1',
          '#1e90ff',
          '#c71585',
          'rgba(255, 69, 0, 0.68)',
          'rgb(255, 120, 0)',
          'hsv(51, 100, 98)',
          'hsva(120, 40, 94, 0.5)',
          'hsl(181, 100%, 37%)',
          'hsla(209, 100%, 56%, 0.73)',
          '#c7158577'
        ]
      }
    }
  };
</script>
```

### 不同尺寸

```javascript
<el-color-picker v-model="color4"></el-color-picker>
<el-color-picker v-model="color4" size="medium"></el-color-picker>
<el-color-picker v-model="color4" size="small"></el-color-picker>
<el-color-picker v-model="color4" size="mini"></el-color-picker>

<script>
  export default {
    data() {
      return {
        color4: '#409EFF'
      }
    }
  };
</script>
```

### Attributes

| 参数         | 说明                      | 类型    | 可选值                | 默认值                                                |
| :----------- | :------------------------ | :------ | :-------------------- | :---------------------------------------------------- |
| disabled     | 是否禁用                  | boolean | —                     | false                                                 |
| size         | 尺寸                      | string  | —                     | medium / small / mini                                 |
| show-alpha   | 是否支持透明度选择        | boolean | —                     | false                                                 |
| color-format | 写入 v-model 的颜色的格式 | string  | hsl / hsv / hex / rgb | hex（show-alpha 为 false）/ rgb（show-alpha 为 true） |
| popper-class | ColorPicker 下拉框的类名  | string  | —                     | —                                                     |
| predefine    | 预定义颜色                | array   | —                     | —                                                     |

### Events

| 事件名称      | 说明                               | 回调参数         |
| :------------ | :--------------------------------- | :--------------- |
| change        | 当绑定值变化时触发                 | 当前值           |
| active-change | 面板中当前显示的颜色发生改变时触发 | 当前显示的颜色值 |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com