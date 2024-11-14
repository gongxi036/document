
#  `Performance` 指标解析

###  关键名称

| 名词 | 全称                 | 解释            |
| ---- | -------------------- | --------------- |
| FP   | firstPaint           | 首次绘制        |
| FCP  | firstContentfulPaint | 首次内容绘制    |
| LCP  | largeContentfulPaint | 最大内容绘制    |
| DCL  | domContentLoaded     | dom内容解析完成 |
| L    | loaded               | 页面的load事件  |

###  DCL (DOMContentLoaded)

> 当初始的 HTML 文档被完全加载和解析完成之后，**`DOMContentLoaded`** 事件被触发，而无需等待样式表、图像和子框架的完全加载。

#####  事件监听

```js
document.addEventListener('DOMContentLoaded', (event) => {
    console.log('DOM 完全加载和解析')
})
```

###  L (Load)

> **`Load`** 事件在整个页面及所有依赖资源(样式表、图片)都已完成加载时触发。它与 **`DoMContentLoadd`** 不同，后者只要页面 `DOM`加载完成就触发，无需等待依赖资源的加载。  

#####  事件监听

```js
window.addEventListener('load', (event) => {
    console.log('页面加载完成')
})
```

####  DCL与 Load 触发的先后顺序

很多人可能认为 `Load` 的触发一定会在 `DCL`之后，虽然绝大多数我们看到的确实是这样，但从两者从[MDN]() 的解析上来看， `DCL` 关注的是 HTML文档的加载与解析，而 `Load` 值关注资源的加载，所以两者触发的先后顺序并不是绝对的。

如果页面非常的简单，没有引入任何的外部资源，则`Load`先行。

**所以两者触发的先后顺序并不是固定的，如果页面有许多外部资源需要加载，那么 `load`事件会后触发；如果页面内容较多，外部资源较少，那么`load`事件可能先触发**

###  FP 与 FCP

> FP 全称 First Paint, 代表 **首次渲染的事件点**，即首次视觉变化发生的时间点。前端开发者经常谈到的 **白屏时间** (用户看不到任何内容) 就是 **用户访问网页 到 FP 的这段时间**

> FCP，全称 First Contentful Paint，代表 **首次DOM内容渲染的时间点**，**DOM内容**可以是文本、图像(包括背景图像)、`<svg>`元素或者非白色的`<canvas>`元素

简单点理解就是 `FCP`事件指渲染出第一个内容的事件，而 `FP`指渲染出第一个像素点，渲染出的东西可能是内容，也可能不是。

**⚠️需要注意的是， `FP`一定不会比 `FCP`晚触发，但可能会一起触发，绝大多数情况是 `FP`在 `FCP`之前触发!**

####  几种场景

**第一种：**无`FP`

页面上有**节点**但没有样式，很显然这种情况是不需要渲染页面的，所以也没有`FP`

> 节点包含是一些不可见的节点，如 `div、section、span`；但是一些有内容的节点除外，如`img、input/video`

还有一种情况就是 **如果要渲染的内容不在视口内的，那么也是不会触发`FP`**

**第二种：**有`FP`无`FCP`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div class="container">
        <input type="text" />
    </div>

</body>
</html>
```

####  `FP`与`DCL`的触发顺序

浏览器不一定等到所有的 DOM 都解析完再开始渲染，如果DOM节点少，浏览器会加载完再去渲染；但是如果节点很多，浏览器解析一部分节点后就会开始渲染(这时候就会触发FP)。也就是说，当需要渲染的节点数少的时候，DCL会在FP前面；当渲染的节点数很多的时候，DCL会在FP后面。

**现在来说，绝大部分的项目都是FP在DCL之前触发，这样用户可以更快的看到页面内容**

###  LCP

> LCP, 全称 Largest Contentful Paint。根据页面首次开始加载的时间点（即 **first started loading**，可以通过`performance.timeOrigin`得到）来报告可视区域内可见的**最大图像或文本块**完成渲染的相对时间

####  LCP 评分

为了提供良好的用户体验，我们应努力将最大内容绘制时间控制在**2.5 秒**或更短。

####  LCP 包含元素

- `<img>`

- `<img>`、`<svg>`元素内的元素

- `<video>`带海报图像的元素

- 具体通过函数加载的背景图像元素`url()`(而不是CSS渐变)

- 包含文本节点或其他内联文本元素子元素的块级元素



###  [参考文献](https://www.cnblogs.com/songyao666/p/17540259.html)

  
