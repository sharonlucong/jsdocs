### Webpack

**概要**：模块打包，分析项目结构，找到js以及浏览器不能直接运行的拓展语言（scss，ts等），将其转换打包为可在浏览器运行的格式

**工作方式**：把项目当作一个整体，通过给定的主文件如index.js, webpack将从这个文件开始找到项目的所有依赖文件，使用loaders处理，打包为一个（或多个）js文件

**安装**:

```
//全局安装
npm install -g webpack
//安装到你的项目目录
npm install --save-dev webpack

```

**练习**：

建立项目结构
```
webpack_practice
  - node_modules
  - app
    - Greeter.js
    - main.js
  - public
    - index.html
  package.json
```

使用webpack

法一：terminal操作
```
//在terminal中使用如下指令

//如果是全局安装webpack
webpack <entry file> <destination for bundle file> //e.g. webpack app/main.js public/bundle.js

//如果是非全局安装
node_modules/.bin/webpack app/main.js public/bundle.js
```

法二：配置文件, 本身是一个js模块，可以把打包相关信息放入
```
//在根目录下新建webpack.config.js文件

module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
}

//“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。
```
配置好后，在terminal执行webpack就可以了。（非全局node_modules/.bin/webpack）

法三：更快捷的打包方式

使用`npm`引导任务执行

```
{
  "name": "webpack-sample-project",
  "version": "1.0.0",
  "description": "Sample webpack project",
  "scripts": {
    "start": "webpack" // 修改的是这里，JSON文件不支持注释，引用时请清除
  },
  "author": "zhang",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^1.12.9"
  }
}
```

**Webpack的强大功能**
- 调试（生成Source Maps － 提供了对应编译文件和源文件的方法）

```
module.exports = {
  devtool: 'eval-source-map', //只有在开发阶段使用
  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  }
}
```

**使用webpack构建本地服务器**
浏览器监听你的代码的修改，并自动刷新显示修改后的结果
```
npm install --save-dev webpack-dev-server
```

```
module.exports = {
  devtool: 'eval-source-map', //只有在开发阶段使用
  entry:  __dirname + "/app/main.js",
  output: {
    path: __dirname + "/public",
    filename: "bundle.js"
  },
  devServer: {
    contentBase: "./public",//本地服务器所加载的页面所在的目录
    historyApiFallback: true,//不跳转
    inline: true//实时刷新
  } //port default is 8080
}
```

设置完之后，在package.json中
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack",
    "server": "webpack-dev-server --open"
  }
```

**Loaders**
通过使用不同的loader，webpack有能力调用外部的脚本或工具，实现对不同格式的文件的处理，比如说分析转换scss为css，或者把下一代的JS文件（ES6，ES7)转换为现代浏览器兼容的JS文件，对React的开发而言，合适的Loaders可以把React的中用到的JSX文件转换为JS文件。

- Babel
Babel是一个编译js的平台，拥有几个模块化的包，常用的为`babel-preset-es2015`和`babel-preset-react`
```
npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
```
```
module.exports = {
    entry: __dirname + "/app/main.js",//已多次提及的唯一入口文件
    output: {
        path: __dirname + "/public",//打包后的文件存放的地方
        filename: "bundle.js"//打包后输出文件的文件名
    },
    devtool: 'eval-source-map',
    devServer: {
        contentBase: "./public",//本地服务器所加载的页面所在的目录
        historyApiFallback: true,//不跳转
        inline: true//实时刷新
    },
    //这个例子允许你使用ES6以及JSX的语法
    module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/, //匹配loaders所处理文件的拓展名的正则表达式
                use: {
                    loader: "babel-loader",
                    options: {
                        presets: [
                            "es2015", "react"
                        ]
                    }
                },
                exclude: /node_modules/
            }
        ]
    }
};
```
- 关于babel的配置
因为babel有非常多配置选项，放在`webpack.config.js`中太过冗余，因次，一般把配置放在一个单独的名为".babelrc"的配置文件中。

比如上例中，拎出 options, webpack会自动调用
```
//.babelrc
{
  presets: [ "es2015", "react"]
}

```

**处理CSS**
webpack中有`css-loader`:使用@import和 url(...)实现require()功能   和`style-loader`： 将所有计算后的样式加入页面

```
npm install --save-dev style-loader css-loader
```
```
module.exports = {
   ...
    module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/,
                use: {
                    loader: "babel-loader"
                },
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: [
                    {
                        loader: "style-loader"
                    }, {
                        loader: "css-loader"
                    }
                ]
            }
        ]
    }
};
```

在main.js中，要引入css文件

```
import "xxx.css"
```

**模块化css**
```
module.exports = {

    ...

    module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/,
                use: {
                    loader: "babel-loader"
                },
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: [
                    {
                        loader: "style-loader"
                    }, {
                        loader: "css-loader",
                        options: {
                            modules: true //这样不必单行在不同的模块中使用相同的类名造成冲突
                        }
                    }
                ]
            }
        ]
    }
};
```

```
import React, {Component} from 'react';
import config from './config.json';
import styles from './Greeter.css';//导入

class Greeter extends Component{
  render() {
    return (
      <div className={styles.root}>//添加类名
        {config.greetText}
      </div>
    );
  }
}

export default Greeter
```

**CSS预处理**
webpack中有一些常见的预处理loaders
- Less Loader
- Sass Loader
- Stylus Loader
- PostCSS

**插件**

`区别`：loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个，插件并不直接操作单个文件，它直接对整个构建过程其作用。

`使用`：使用插件，首先用`npm`进行安装，然后在webpack配置中plugins关键字部分添加该插件的一个实例。

```
const webpack = require('webpack');

module.exports = {
  ...
  module: {

  },
  plugins: [
    new webpack.BannerPlugin('版权所有')
  ]
}
```

`常用插件`：

`html-webpack-plugin`:  index.html会自动生成；在app目录下需要创建index.tmpl.html的模版

`Hot Module Replacement`

`优化类插件`：
`OccurenceOrderPlugin`：为组建分配id

`UglifyJsPlugin`：压缩js

`ExtractTextPlugin`: 分离js和css

**缓存**
包括[name],[id],[hash]

```
module.exports = {
..
    output: {
        path: __dirname + "/build",
        filename: "bundle-[hash].js"
    },
   ...
};
```

大部分引用自：

作者：zhangwang
链接：http://www.jianshu.com/p/42e11515c10f

[Ref](http://www.jianshu.com/p/42e11515c10f)