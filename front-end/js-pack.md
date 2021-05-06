# js 构建打包工具优化及未来

是期待 webpack 的迭代优化还是期待次世代的构建工具？

### HappyPack

* [https://github.com/amireh/happypack\#how-it-works](https://github.com/amireh/happypack#how-it-works)
* [https://www.npmjs.com/package/happypack](https://www.npmjs.com/package/happypack)
* 由于 thread-loader 出现，作者已经表示 interest in the project is fading away

目的：解决 webpack 打包速度慢的问题

实现：HappyPack 把任务分解给多个子进程去并发的执行，子进程处理完后再把结果发送给主进程。

使用 happypack loader 包裹其它 loader

```javascript
const HappyPack = require('happypack')

exports.module = {
    rules: [
        {
            test: /.js$/,
            // 1) replace your original list of loaders with "happypack/loader":
            // loaders: [ 'babel-loader?presets[]=es2015' ],
            use: 'happypack/loader',
            include: [
                /* ... */
            ],
            exclude: [
                /* ... */
            ],
        },
    ],
}

exports.plugins = [
    // 2) create the plugin:
    new HappyPack({
        // 3) re-add the loaders you replaced above in #1:
        loaders: ['babel-loader?presets[]=es2015'],
    }),
]
```

* id: String 用唯一的标识符 id 来代表当前的 HappyPack 是用来处理一类特定的文件.
* loaders: Array 用法和 webpack Loader 配置中一样.
* threads: Number 代表开启几个子进程去处理这一类型的文件，默认是 3 个，类型必须是整数。

  verbose: Boolean 是否允许 HappyPack 输出日志，默认是 true。

* threadPool: HappyThreadPool 代表共享进程池，即多个 HappyPack 实例都使用同一个共享进程池中的子进程去处理任务，以防止资源占用过多。

  verboseWhenProfiling: Boolean 开启 webpack --profile ,仍然希望 HappyPack 产生输出。

* debug: Boolean 启用 debug 用于故障排查。默认 false。

### thread-loader

* 整体与 happypack 相似，建议小项目不用，worker 开启有时间消耗

```javascript
module.exports = {
    module: {
        rules: [
            {
                test: /\.js$/,
                include: path.resolve('src'),
                use: [
                    {
                        loader: 'thread-loader',
                        // 有同样配置的 loader 会共享一个 worker 池(worker pool)
                        options: {
                            // 产生的 worker 的数量，默认是 cpu 的核心数
                            workers: 2,

                            // 一个 worker 进程中并行执行工作的数量
                            // 默认为 20
                            workerParallelJobs: 50,

                            // 额外的 node.js 参数
                            workerNodeArgs: ['--max-old-space-size', '1024'],

                            // 闲置时定时删除 worker 进程
                            // 默认为 500ms
                            // 可以设置为无穷大， 这样在监视模式(--watch)下可以保持 worker 持续存在
                            poolTimeout: 2000,

                            // 池(pool)分配给 worker 的工作数量
                            // 默认为 200
                            // 降低这个数值会降低总体的效率，但是会提升工作分布更均一
                            poolParallelJobs: 50,

                            // 池(pool)的名称
                            // 可以修改名称来创建其余选项都一样的池(pool)
                            name: 'my-pool',
                        },
                    },
                    'expensive-loader',
                ],
            },
        ],
    },
}
```

* 可以通过预热 worker 池\(worker pool\)来防止启动 worker 时的高延时。

```javascript
const threadLoader = require('thread-loader')

threadLoader.warmup(
    {
        // pool options, like passed to loader options
        // must match loader options to boot the correct pool
    },
    [
        // modules to load
        // can be any module, i. e.
        'babel-loader',
        'babel-preset-es2015',
        'sass-loader',
    ],
)
```

### TypeScript to JavaScript

* tsc：typescript 官方编译 cli
* ts-loader：使用的是 tsc，与 webpack 集成使用
* aswsome-typescript-loader：第三方提供的 webpack ts编译插件，弥补 tsc 的一些不足
* babel-loader：简单粗暴，去除 ast 树上的所有 type 信息，留下 js

### snowpack

* [https://www.snowpack.dev/](https://www.snowpack.dev/)
* 解决开发过程中 webpack 打包太慢的问题

![](../.gitbook/assets/snowpack-unbundled-example-3.png)

基本实现原理：

1. 单独对每个文件进行编译，生成 js 文件直接挂在文件服务上
2. 使用 ES6 module script 在浏览器中引入入口文件 e.g. `<script type="module" src="/_dist_/index.js"></script>`
3. 在 index.js 内使用 ES6 modules
4. 提供浏览器热更新

```javascript
// at _dist_/index.js
import React from '/web_modules/react.js'
import ReactDOM from '/web_modules/react-dom.js'
import App from './App.js'
import './index.css.proxy.js'
```

* 对于 node\_mudules 放到 ./web\_modules 下
* 对于 css 生成 \*.css.proxy.js ，用 esm 引入

  ```javascript
    // ./index.css.proxy.js
    const code = "body {\n  margin: 0;\n  font-family: ...";
    const styleEl = document.createElement("style");
    const codeEl = document.createTextNode(code);
    styleEl.type = 'text/css';
    styleEl.appendChild(codeEl);
    document.head.appendChild(styleEl);
  ```

* 对于资源文件

  ```javascript
    // ./logo.svg.proxy.js
    export default "/_dist_/logo.svg";
  ```

### vite

原理与 snowpack 一致，针对 vue 项目开发，不展开讨论

* [https://github.com/vitejs/vite](https://github.com/vitejs/vite)

