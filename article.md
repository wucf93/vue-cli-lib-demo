本文会探讨一下如何利用vue-cli快速构建一个库的工程化环境。

## 前言
当我们需要做一个库的开发时，我们可能会这样做：

- 编写两套`webpack.config.js`文件，一个是dev，一个prod
- 添加`webpack`，`entry`，`output`...等配置
- 添加`babel插件`,`less-loader`,`vue-loader`,`jsx-loader`,`ts-loader`...
- 提取两套config中共同的配置，然后通过`wepack.merge()`加入
- 配置webpack的`lib`，`umd,cmd,es`...
- webpack优化，`import()`,`压缩`...
- ...

可见，我们做一个具有工程能力的库多么麻烦，特别是一些小功能的库的时，可能webpack配置的部分比我们功能开发还要久。

现在，有一种更好的方法，让我们能快速进行原型开发。

## vue-cli

`vue-cli`我们一般主要用来构建vue项目的脚手架，但其实它还包含很多有用的功能，比如说快速原型构建，下面是官网的截图：

![](https://user-gold-cdn.xitu.io/2020/5/18/172270cd37d76df5?w=1600&h=474&f=png&s=221590)

> 注：官网上说对`*.vue`，其实对`*.js`也是有效的。 

由上面的截图可以看到，主要依赖于`vue serve`和`vue build`两个命令。下面来看看这两个命令的功能吧。

### vue serve

```bash
Usage: serve [options] [entry]
在开发环境模式下零配置为 .js 或 .vue 文件启动一个服务器

Options:
  -o, --open  打开浏览器
  -c, --copy  将本地 URL 复制到剪切板
  -h, --help  输出用法信息
```

### vue build

```bash
Usage: build [options] [entry]
在生产环境模式下零配置构建一个 .js 或 .vue 文件

Options:
  -t, --target <target>  构建目标 (app | lib | wc | wc-async, 默认值：app)
  -n, --name <name>      库的名字或 Web Components 组件的名字 (默认值：入口文件名)
  -d, --dest <dir>       输出目录 (默认值：dist)
  -h, --help             输出用法信息
```

上面的api已经很详细了，下面来看下demo

## demo

先看一下目录结构
```
    ｜-- dist            构建后的目录
    ｜-- examples        开发
        ｜-- index.js    
    ｜-- src             源码
        ｜-- main.js
        ｜-- util.js
    ｜-- package.json
    ｜--README.md
```

#### package.json

```json
    {
        "name": "vue-cli-lib-demo",
        "version": "1.0.0",
        "description": "利用vue-cli创建库的demo",
        "main": "dist/vue-cli-lib-demo.cjs.js",
        "module": "dist/vue-cli-lib-demo.esm.js",
        "browser": "dist/vue-cli-lib-demo.umd.js",
        "scripts": {
            "serve": "vue serve examples/index.js",
            "build": "vue build --target lib --name vue-cli-lib-demo src/main.js"
        },
        "author": "wucf",
        "license": "MIT",
        "devDependencies": {
            "@vue/cli": "^4.3.1",
            "@vue/cli-service": "^4.3.1",
            "@vue/cli-service-global": "^4.3.1"
        }
    }
```

#### main.js

```js
    import { numberAdd } from "./util";

    export default {
        add(num1, num2) {
            return numberAdd(num1, num2);
        },
    };
```

当执行`npm run build`之后，就会生成一个`dist`目录，里面就有打包好的库。

![](https://user-gold-cdn.xitu.io/2020/5/19/1722c8e81f4805c2?w=680&h=394&f=png&s=222074)

然后npm publish 搞定！

如果还不满足，同样可以配置`vue.config.js`进行自定义的配置。

## 小结

这种方案不是最优的，但是是脑力负担最小的，特别是项目周期特别短的情况下，更需要我们这种**银弹**来快速解决问题。

各位大佬，点赞小心心走起！



