
<p align="center">
  <a target="_blank" href="https://jshub.cn/">
  <img alt="special sponsor appwrite" src="https://raw.githubusercontent.com/995231030/autofit.js/master/autofit.png" width="300">
  </a>
</p>

<p align="center">
  <h3 align="center">autofit.js</h3>
</p>

<center><font face="黑体" size=26></font></center>



简体中文 | [English](./readme.en.md)

autofit.js是一个可以让你的PC项目自适应屏幕的工具，其原理非常简单，即在scale等比缩放的基础上，向右或向下增加了宽度或高度，以达到充满全屏的效果，使用autofit.js不会挤压、拉伸元素，它只是单纯的设置了容器的宽高。




| 时间       | 版本   | 描述                                              |
| ---------- | ------ | ------------------------------------------------- |
| 2023-04-16 | v1.0.0 | 第一个版本发布 🥳                                  |
| 2023-04-23 | v1.0.9 | 解决最大化、f11全屏后失效的问题                   |
| 2023-05-12 | v1.1.2 | 新增忽略元素功能（地图热区事件偏移解决）👍         |
| 2023-05-22 | v2.0.0 | 新增关闭autofit影响选项                           |
| 2023-05-30 | v2.0.2 | 增加兼容性，新增错误提示                          |
| 2023-06-07 | v2.0.3 | 新增延迟、过渡、图表适配                          |
| 2023-06-19 | v2.0.5 | 修复多个ignore不生效的问题，默认关闭延迟和过渡    |
| 2023-07-04 | v2.0.6 | 未发布-修复无法正常off的问题，新增ts参数undefined |
| 2023-07-11 | v3.0.0 | 提升稳定性，提升易用性                            |

### v3.0.0 新版介绍

igonre 现在可以传入 Array\<string> 了 ，即：

```js
autofit.init({
	ignore:['.leaflet','.gaodemap']
})
```

现在ignore的宽高支持其他单位，即：

```js
autofit.init({
	ignore:[{
    	el:'.gaodemap',
        width:"80%",
        height:'200px'
    }]
})
```

在v3.0.0(包括)之后，参数将不再兼容designWidth 、designHeight 、renderDom ，字段变化请看下文。在ignore中，依然兼容 el 、dom参数两种写法（以及字符串写法）。



字段变化：`designWidth > dw ` `designHeight > dh` `renderDom > el`

v2.0.5 版本是最后一个兼容版本，之后将只支持新字段

安装旧版：

```shell
npm i autofit.js@2.0.5
```



### autofit.js

autofit.js 这是一款可以使你的项目一键自适应的工具

理论上可以支持从你的设计稿以下的分辨率。

### 引入

```js
import autofit from 'autofit.js'
```

### 快速开始

```js
autofit.init()
```

> 默认参数为1920*929（即去掉浏览器头的1080）, 直接调用即可

### 使用

```js
export default {  
  mounted() {
	autofit.init({
        dh: 1080,
        dw: 1920,
        el:"#app",
        resize: true
    },false) // 可关闭控制台运行提示输出
  },
}
```

> 以上使用的是默认参数，可根据实际情况调整，可选参数有
>
> ```js
>    * - el：渲染的dom，默认是 "#app"，必须使用id选择器 
>    * - dw：设计稿的宽度，默认是 1920 
>    * - dh：设计稿的高度，默认是 929 ，如果项目以全屏展示，则可以设置为1080
>    * - resize：是否监听resize事件，默认是 true
>    * - ignore：忽略缩放的元素（该元素将反向缩放），参数见readme.md
>    * - transition：过渡时间，默认是 0
>    * - delay：默认是 0
> 
> ```

### 忽略某些元素

```js
autofit.init({
  ignore: [
     { 
      el: ".gaodeMap",
     },
  ]
})
```

传入 `ignore` 以使元素不被缩放

更个性化的设置：

```js
autofit.init({
  ignore: [
    {
      el: ".gaodeMap", //必填
      height: "300px",//可选，需注明单位
      width: "80%",//可选，需注明单位
      scale:1.2, //可选：回放程度，基于主元素缩放后的大小
      fontSize:26 //可选，如果自定义缩放后文字大小不合适，可以在这里设置文字大小
    },
    {
        //...
    }
  ]
})
```

如果反向缩放后的元素大小使结构发生变化，你还可以手动传入宽高、回放程度。

ignore还支持传入字符串数组：

```js
autofit.init({
  ignore: ['.gaodeMap','.leaflet','#someElementClassOrId']
})
```



### elRectification

一些canvas渲染的图表也会有事件偏移，而图表不同于地图，当使用ignore时，过大的图表可能会显示不全，因此可以使用`elRectification` （性能上不如ignore） 

如果ignore无法满足需求，可以使用 `autofit.elRectification(".classNameOrId")`

```js
import { elRectification } from 'autofit.js'
```

```html
<div class="G2plot"></div>
<div class="G2plot"></div>
<div class="G2plot"></div>
```

```js
onMounted(() => {
  elRectification(".G2plot")
})
```

使用 elRectification 时，需要被矫正的元素已经挂载

### 关闭 autofit.js造成的影响

当autofit未初始化时，会出现无法找到元素的错误，在使用autofit.off()前，请确保已初始化。

```js
autofit.off()
```

