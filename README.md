###  1. 解构

###  2. `import and export`

###  3.数组的循环方式以及封装：`map`，`for ...in, for...of`

###  4. `image-webpack-loader` -- 对图片进行压缩

```javascript
{
  test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
  use:[
    {
    loader: 'url-loader',
    options: {
      limit: 10000,
      name: utils.assetsPath('img/[name].[hash:7].[ext]')
      }
    },
    {
      loader: 'image-webpack-loader',
      options: {
        bypassOnDebug: true,
      }
    }
  ]
}
```

###  5.类型判断的方法

- `typeof`
- `instanceof`
- `Object.prototype.toString.call()`

###  6.`call`和`apply`的实现 （`bind`）

​	[参考地址](https://segmentfault.com/a/1190000020044435)

###  7.编程

1. 查找字符串中出现最多的字符和个数

   例: `abbcccddddd` -> 字符最多的是d，出现了5次

2. 字符串查找

   请使用最基本的遍历来实现判断字符串 a 是否被包含在字符串 b 中，并返回第一次出现的位置（找不到返回 -1）

   ```javascript
   a='34';b='1234567'; // 返回 2
   a='35';b='1234567'; // 返回 -1
   a='355';b='12354355'; // 返回 5
   isContain(a,b);
   
   function isContain(a, b) {
     for (let i in b) {
       if (a[0] === b[i]) {
         let tmp = true;
         for (let j in a) {
           if (a[j] !== b[~~i + ~~j]) {
             tmp = false;
           }
         }
         if (tmp) {
           return i;
         }
       }
     }
     return -1;
   }
   ```

   

3. 实现千位分隔符

   ```javascript
   // 保留三位小数
   parseToMoney(1234.56); // return '1,234.56'
   parseToMoney(123456789); // return '123,456,789'
   parseToMoney(1087654.321); // return '1,087,654.321'
   
   function parseToMoney(num) {
     num = parseFloat(num.toFixed(3));
     let [integer, decimal] = String.prototype.split.call(num, '.');
     integer = integer.replace(/\d(?=(\d{3})+$)/g, '$&,');
     return integer + '.' + (decimal ? decimal : '');
   }
   ```



###  8. [通信协议](https://segmentfault.com/a/1190000019891825)

 	介绍了 `http1.0 http1.1 http2.0 https websocket`等协议

###  9. [前端性能优化不完全手册](https://segmentfault.com/a/1190000018827395)

###  10. [前端开发者必备的`Nginx`知识](https://juejin.im/post/5c85a64d6fb9a04a0e2e038c#)

###  11. 什么是`BFC`， `BFC`用来解决什么问题

###  12. [前端十大经典算法](https://segmentfault.com/a/1190000020072884)

###  13. [webpack 4 入门](https://segmentfault.com/a/1190000020063707)

###  14. `Webpack4` 三部曲

​	[实战一](https://juejin.im/post/5d520c01e51d453b1e478a97)

​	[实战二](https://juejin.im/post/5d59f484e51d453bc5231627)

​	[实战三](https://juejin.im/post/5d59f94f6fb9a06b0c0865b1)















