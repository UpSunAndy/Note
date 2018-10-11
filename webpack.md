## 1.0 webpack
+ 打包资源的工具：
  - 将所有的资源打包到js中
  - 将所有的sass,lesss打包到css文件中
  - 将所有的字体打包到字体文件中
### 1.1 webpack的简单使用
+ a.全局安装
  + npm i webpack -g
+ b.测试是否安装成功
  + webpack -v
+ c.安装一个依赖项目：webpack-cli
  + npm i webpack-cli -g
+ d.打包项目
  + 指令：
    - webpack 入口文件
    - 将们书写的main.js打包到dist目录下生成一个main.js文件
### 1.2 webpack的打包
+ a. 本地安装
  + 直接将webpack当作第三方包安装到当前项目中
    - npm install webpack webpack-cli
+ b. 配置所有的打包项从webpack.config.js中的去找
  ```package.json
    "script": {
      build: "webpack --config webpack.config.js"
    }
  ```
+ c. 创建一个webpack.config.js
  - 入口(entry)
    - entry属性：
  - 输出(output)
    - output属性：
  - 模式(mode)
  - loader
    - 加载资源：
      - 加载css资源
        - style-loader, css-loader
      - 加载less资源
        - style-loader, css-loader, less-loader
      - 加载sass资源
        - style-loader, css-loader, sass-loader
      - 加载图片资源
        - file-loader
  - 插件(plugins)
    + html-webpack-plugin：自动生成html入口文件
    + clean-webpack-plugin： 自动清除dist目录
+ d. 将所有的打包依赖项都配置到webpack.config.js中
  - 入口 
  - 出口
+ e. 打包
  - 打包js代码（webpack自带的功能）
    + 1）在webpack打包过程中可能将node中的代码打包为浏览器认识的代码
    + 2）会以入口文件为主将所有入口文件中引入的文件进行打包（打包到同一个js文件中）
    + 将es6语法转为es5语法
      + 下载第三方包：babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env
      ```
      npm i babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env
      ```
      + 在webpack.config.js配置打包信息
      ```
        {
          test: /\.js$/, // 正则，用来判断文件是否是js后缀
          // 不转换node_moudels中的js文件 
          exclude: /(node_modules)/,
          use: {
            loader: 'babel-loader',
            options: {
              presets: ['@babel/preset-env']
            }
          }
        }
      ```
  - 打包css代码
    + 下载loader

      - npm install style-loader css-loader
    + 配置配置文件
      ```
      +   module: {
      +     rules: [
      +       {
      +         test: /\.css$/,
      +         use: [
      +           'style-loader',
      +           'css-loader'
      +         ]
      +       }
      +     ]
      +   }
      ```
    + 使用import './assets/index.css'来引入css文件 
  - 打包less代码
    + 安装 less-loader, less

      - npm i less-loader less
    + 配置 webpack.config.js文件
      ```
         {
        test: /\.less$/,
        use: [
          { loader: 'style-loader' }, // 将打包的css代码放到页面上用style标签包裹起来
          { loader: 'css-loader' }, // 打包解析好的less方法
          { loader: 'less-loader' } // 将less语法打包为css语法
        ]
      }
      ```
    + 使用import 导入less文件
      ```
        import "./assets/less.less";
      ```
  - 打包sass代码
    + 安装 sass-loader node-sass

      - npm i sass-loader node-sass
    + 配置 webpack.config.js文件
      ```
         {
        test: /\.scss$/,
        use: [
          { loader: 'style-loader' }, // 将打包的css代码放到页面上用style标签包裹起来
          { loader: 'css-loader' }, // 打包解析好的less方法
          { loader: 'sass-loader' } // 将less语法打包为css语法
        ]
      }
      ```
    + 使用import 导入less文件
      ```
        import "./assets/sass.scss";
      ```
  - 打包图片
    + 下载loader: file-loader
      ```
      npm install file-loader
      ```
    + 配置webpack.config.js
      ```
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: [
          'file-loader'
        ]
      }
      ```
    + 使用图片
  - 打包字体
    + 下载loader第三方包：file-loader

      - 由于在打包图片时已经将这个第三方包下载了，所以不需要再次下载
    + 配置config.js文件
      ```
        {
          test: /\.(woff|woff2|eot|ttf|otf)$/,
          use: [
            'file-loader'
          ]
        }
      ```
+ f. 设定HtmlWebpackPlugin(设置打包的插件的)
  - 1.0 自动生成index.html
    + 下载需要的第三方包：html-webpack-plugin
    + 配置config.js文件
      ```
        1.0 引用第三方包
        const HtmlWebpackPlugin = require('html-webpack-plugin');
        2.0 设置配置项
        plugins: [
          new HtmlWebpackPlugin({
            title: 'Output Management'
          })
        ],
      ```
  - 2.0 自动清除dist目录中的内容
    + 下载一个自动清除dist文件的插件： clean-webpack-plugin
      ```
        npm i clean-webpack-plugin
      ```
    + 需要到配置文件中的插件属性中配置这个第三方包
      ```
        1.0 引入自动清除的插件
        const CleanWebpackPlugin = require('clean-webpack-plugin');
        2.0 创建这个插件对象
        new CleanWebpackPlugin(['dist'])
      ```
## 2.0 webpack-dev-server 工具的使用
+ 将我们书写的网站以服务器的方式打开，修改代码之后就不再需要再次打包了
+ a. 下载第三方包：  webpack-dev-server
  ```
    npm i webpack-dev-server
  ```
+ b. 去config.js文件中的去配置
  ```
    devServer: {
      contentBase: './dist'
    }
  ```
+ c. 去package.json 中去配置
  ```
    "script": {
      "start": "webpack-dev-server --open"
    }
  ```
## 3.0 模块热替换(HMR)
+ 它允许在运行时更新各种模块，而无需进行完全刷新。
+ 用法：
  - 配置config.js文件
    ```
      // 1.0 引入webpack
      const webpack = require('webpack');
      // 2.0 设置热更新
      devServer {
        hot: true 
      }
      // 3.0 添加两个插件：
      new webpack.NamedModulesPlugin(),
      new webpack.HotModuleReplacementPlugin()
    ```
## 4.0 source-map 设置错误映射代码源
+ devtool
  + source-map： 单独创建一个map文件，用来映射错误代码
  + inline-source-map：直接将文件的映射放到文件中
## 5.0 使用resolve配置相关信息：
+ extensions:
  - 可以设置引入文件时可省略的后缀
+ alias：
  - 可以设置别名
## 6.0 在webpack中打包.vue后缀的文件
+ 使用vue-loader
+ 步骤：
  - 下载 vue-loader
  - 配置：
    + 在打包规则中要中入 .vue后缀的判断
    + 在插件中也加入对象
    + 以上代码在：https://vue-loader.vuejs.org/zh/guide/#%E6%89%8B%E5%8A%A8%E9%85%8D%E7%BD%AE
  - 问题：
    - 报错1：
      + 再下载包： vue-template-compiler
    - 报错2：
## 7.0 JSONP的实现原理
+ 作用：解决跨域问题
  - 跨域：协议/ip地址/端口号
  - 跨域本质是浏览器的行为不是服务器的行为
    + 浏览器虽然存在跨域，但是script标签不存在跨域

## 8.0 原型连：
+ https://www.cnblogs.com/shuiyi/p/5305435.html
+ 原型：
  + prototype
    + 属于构造函数
  + _proto_
    + 属性对象

## 9.0 vue-cli
+ 2.0的使用
  + 安装：
    - npm i -g vue-cli
    - npm install -g @vue/cli-init
  + 生成项目 
    - vue init webpack 项目名
+ 3.0 的使用
  + 安装：
    - npm i -g vue-cli
  + 生成项目
    - vue create new-app
  + vue-ui