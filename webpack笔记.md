# webpack笔记



### 概念

webpack是一个现代JS应用程序的静态魔偶快打包器。当webpack处理应用程序时，他会递归地构建一个依赖关系图，其中包含应用程序需要的每个模块，然后将这些模块打包成一个或多个bundle。



### webpack的四个核心概念

 ·**入口**

·**出口**

·**loader**

·**插件**



#### 入口（entry）

​	指定某个模块作为内部依赖图的开始，进入入口七点后，webpack会找出那些模块和库是入口起点（直接/间接）依赖的，每个依赖项随即呗处理，最后输出到称之为bundles的文件中。

可以通过在webpack配置中配置entry属性，来指定一个入口起点（可以是多个起点）。默认值是

**./src** 。

```js
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```



#### 出口（output）

​	output属性告诉webpack将他自己创建的bundles输出到哪里，以及如何命名这些文件，默认值为**./dist** 。基本上，真个应用程序结构都会呗编译到你指定的输出路径的文件夹中，可以通过在配置中之定义个output字段来配置这些处理过程。

```js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};

```



```js
//以我现在的能力，看这部分知识有点吃力，暂时搁置。等我node阶段的学习完成之后再回来继续学习！
//https://www.webpackjs.com/concepts/hot-module-replacement/
```





















