

### 0.1 vue的调试检查工具：
+ 可以用来调试检查Vue对象
+ 安装
    - 直接去google的应该商店去安装（有一个科学上网的方式）
    - 可以使用github中下载第三方包来自己安装：
        + 1.0 找到github中的第三方包的地址
        + 2.0 使用git 克隆下来
        + 3.0 通过npm install 下载所需的第三方包
        + 4.0 通过 npm build指令来进行编译
        + 5.0 打开google浏览器扩展程序，选择开发者模式
        + 6.0 将编译好的插件导入
### 0.2 MVVM的设计思想
+ 所有的数据操作都交VM对象（var vm = new Vue()）
+ 不再关注dom操作
+ 所有的dom操作都由vue封装好，我们将来要操作只需要使用指令
### 0.3 vue相关知识
+ 指令
+ 过滤器
    - 可以用来过滤字符
    - 全局
        + Vue.filter(filterName, function(val){})
    - 局部
        + new Vue({filters: {}})
+ 计算属性
    - 可以根据已有的属性来设置一个新的属性，
+ 使用ref来操作dom
    - 步骤
        + 给要操作dom元素加一个ref属性
        + 通过this.$refs.属性名可以操作dom元素
+ 自定义指令
    - 可以在vue提供的已有的指令的基础再扩展新的指令
    - 全局
        ```
            Vue.directive(directiveName, {
                inserted(el, binding) {
                    // el 是使用这个指令的元素
                    // binding 给这个指令赋的值
                }
            })
        ```
    - 局部
        ```
            new Vue({
                directives: {
                    directiveName: {
                        inserted(el, binding) {
                            // el 是使用这个指令的元素
                            // binding 给这个指令赋的值
                        }
                    }
                })
            })
        ```
### 0.4 json-server/postman
+ json-server
    + 用来快速搭建服务器
    + 步骤：
        - 安装：npm install json-server -g
        - 数据库：db.json
            + 这个json文件中的数据必须是json格式的
            ```
                {
                    "banner": [
                        {id: 1, name: "小米"},
                        {id: 2, name: "大米"},
                    ]
                }
            ```
        - 使用json-server将数据库运行来
            - json-server --watch db.json --port 3004
    + 使用完成以后会自动开启服务器，接收以下API
        - 查询
            + http://localhost:3004/banner get 查询所有数据
            + http://localhost:3004/banner/100 get 根据id查询数据
        - 新增
            + http://localhost:3004/banner post 新增数据（必须要转入数据）
        - 删除
            + http://localhost:3004/banner/100 delete 删除数据
        - 模糊查询
            + http://localhost:3004/banner/name_like=米 get 按条件来查询
### 0.5 axios
+ 用来发送异步请求
+ 请求：
    - axios.get(url).then()
    - axios.post(url, {}).then()
    - axios.delete(url).then()
### 0.6使用json-server + axios完成对案例的增删查（完善）
+ 完善搜索功能
    - 1.0 监控搜索输入框中的内容发生改变
    - 2.0 每当数据发生变化就修改页面上的源数据
    - 3.0 由于源数据来源于服务器，所以我们相当于只能重新发送请求去请求源数据
    - 4.0 如果要操作建议使用侦听器来进行操作

## 1.0 侦听器
+ 可以用来监控一个已经有的属性，如果这个属性发生改变就执行后面的回调函数
+ 步骤：
    - 在vue对象中添加一个watch属性：
    ```
        var vm = new Vue({
            el: "#app",
            data: {
                msg: ""
            },
            watch: {
                msg() {
                    console.log('msg改变了');
                }
            }
        });
    ```
## 2.0 vue中的过渡和动画
+ 准备知识：如果我们使用v-show/v-if来控制元素的显示和隐藏，会触发vue中的过渡和动画
+ 动画设置方式1:
    - 使用enter和leave实现动画
        - 将隐藏/显示的元素放在transition标签中
        - 分别设置状态
            - 在 v-enter/v-leave 中设置 显示/隐藏 之前的状态
            - 在 v-enter-to/v-leave-to 中设置 显示/隐藏 之后的状态
            - 在 v-enter-active/v-leave-active 中设置 显示/隐藏过程中的状态
+ 动画的使用方式2：
    - 使用vue结合animate.css实现动画
        - 将隐藏/显示的元素放在transition标签中
        - 引入animate.css文件
        - 在属性 leave-active-class/enter-active-class 中设置 显示/隐藏 的动画和过渡
        ```
            leave-active-class="animated fadeOutRight"
            enter-active-class="animated fadeInRight"
        ```
+ 动画的使用方法3：
    - 使用js的钩子函数实现动画：
        - 将隐藏/显示的元素放在transition标签中
        -  给transition标签绑定事件：
        ```
            元素显示之前的状态代码
            v-on:before-enter="beforeEnter"
            元素显示之中的状态代码
            v-on:enter="enter"
            元素显示之后的状态代码
            v-on:after-enter="afterEnter"

            元素隐藏之前的状态代码
            v-on:before-leave="beforeLeave"
            元素隐藏之中的状态代码
            v-on:leave="leave"
            元素隐藏之后的状态代码
            v-on:after-leave="afterLeave"
        ```
+ 一个页面多个动画的实现：
    + 给不同的transition属性设置不同的name属性
        - 相应的enter和leave变为这个name属性开头
            - 如果：name-enter , name-enter-to, name-enter-active
## 3.0 组件(重点)
+ 什么是组件：
    - 在vue中每个页面都是由许多的组件组成的，每个组件有自己的功能。
    - 注意：页面上一个vm对象就是一个组件
+ 定义组件：
    - 1.0 创建一个component对象
    - 2.0 使用组件:直接将定义的组件名当作用标签来使用就可以了
    - 注意点：
        + 使用组件时要将大写字母改为小写，并且在前面加上-
        + 组件中的tamplate属性必须有一个根元素，否则会报错
+ 组件中的属性
    + 说明：其实一个组件就是一个vue对象
    + 每个vue对象都有以下属性：
        - data
            ```
                data: function(){
                    return {
                        text: "text"
                    }
                }
            ```
        - methods：与vue对象中的一样
+ 分类 
    - 全局组件
        ```
            Vue.component("mycomponent", {
                template: `<div>我是一个全局组件</div>`
            })
        ```
    - 局部组件
        ```
            var vm = new Vue({
                components: {
                    "mycom": {
                        template: `<div>
                            我是myCom<br/>
                            <input type="text" v-model="txt"><br/>
                        </div>`,
                        data() {
                            return {
                                txt: "mycomTxt"
                            }
                        }
                    }
                }
            }) 
        ```
+ 父组件传参给子组件
    - 在父组件中定义一个变量
        ```
            var vm = new Vue({
                el: "#app",
                data: {
                    msg: "我是父组件中的msg",
                    txt: "txt"
                }
            }
        ```
    - 通过v-bind将数据绑定到子组件中
        ```
            <mycom v-bind:abc="msg" v-bind:def="txt"></mycom>
        ```
    - 子组件要通过props来接收：
        ```
             mycom: {
                template: `<div>
                    我要输出父元素中的msg： {{abc}}, {{def}}
                </div>`,
                props: ["abc", "def"]
            }
        ```
## 4.0 生命周期（重点，难点）
+ 树叶生命周期
    - 春
        - 发芽
    - 夏
        - 成长
    - 秋
        - 掉落
    - 冬 
        - 腐烂
+ vue生命周期
    - var vm = new Vue(); // 开启了一段生命周期
        - 初始化vue对象（创建出来的vue对象是一个空对象，它里面什么都没有）
        - 将数据渲染到vue对象操作的dom节点上（将数据替换掉app中的指令）
        - 当数据发生改变时，将新的数据渲染到dom节点上（更新）
        - vue对象被销毁时（销毁）
    - 钩子函数
        - 1.0 beforeCreate:
            + 在Vue对象初始化完成之前执行
            + 这个函数在执行时，vue对象还不存在 el,data,methods属性
        - 初始化
        - 2.0 created: *********
            + 在vue对象初始化之后执行
            + 这个函数在执行时，vue对象中已经有el,data,methods属性
        - 3.0 beforeMount:
            + 在页面渲染之前执行
            + 在这个函数中是无法得到this.$refs中的内容的
        - 渲染
        - 4.0 mounted *********
            + 在页面渲染之后执行
            + 在这个函数中可以操作this.$refs
        - 5.0 beforeUpdate
            + 在更新之前执行
        - 更新
        - 6.0 updated
            + 在更新之后执行
        - 7.0 beforedestroy
            + 在vue对象被销毁之前执行
        - 销毁
        - 8.0 destroyed()
            + 在vue对象被销毁之后执行
    - 生命周期流程
        + new Vue();
            - 初始化事件和生命周期
            - beforeCreate
            - 创建vue对象，向对象中注册属性和方法
            - created
            - 判断 vm对象中是否有el属性
                - 有
                   + 再判断是否有template发展
                        - 有
                            + 将template中的内容进行渲染，渲染为虚拟dom（虚拟dom：生成页面结构不显示在页面）
                        - 没有
                            + 将 el 属性对应的内容进行渲染，渲染为虚拟dom
                - 没有
                    - 会将这个vue对象当作组件来解决（根vue对象的是一样的），生成虚拟dom
            - beforeMount
            - 渲染：将虚拟dom渲染到页面上
            - mounted
            - 等待数据更新
                - beforeUpdate
                - 数据更新
                - updated
                - 再次渲染页面
                - 等待数据更新
            - beforedestroy
            - 销毁
            - destroyed
## 5.0 单页应用：
+ 单页应用(single page web application，SPA)，是在一个页面完成所有的业务功能，浏览器一开始会加载必需的HTML、CSS和JavaScript，之后所有的操作都在这张页面完成，这一切都由JavaScript来控制。
+ 优点：
    - 操作体验流畅
    - 完全的前端组件化
+ 缺点：
    - 首次加载大量资源(可以只加载所需部分)
    - 对搜索引擎不友好
    - 开发难度相对较高

## 6.0 vue的插件vue-router（重点）
+ vue-router是用来给vue的单页应用设置路由
+ 使用步骤：
    - 1.0 引入vue-router.js文件
    - 2.0 js代码
        + 创建路由对应的组件
        + 创建一个VueRouter对象（就是设置这个对象中的routes属性）
        + 将VueRouter对象创建出来的实例注册给Vue对象
    - 3.0 html代码
        + 在页面上放几个a标签，在a标签中使用之前注册好的路由
        + 在合适的位置放上一个将来渲染数据的标签：<router-view></router-view>
    - 实现原理：
        - 当浏览器使用到vue-router中的路由时，vue会去vue对象中的router属性中找对应路由，找到以后会根据这个路由找到它的component（组件），根据组件中的template属性渲染页面，将渲染的结果显示在router-view标签

## 6.0 vue-cli:vue的项目搭建工具（项目的脚手架）（重点）
+ 帮助我们搭建项目结构
+ 使用vue-cli2.0版本搭建项目结构
    - 1.0 安装：npm install -g @vue/cli
        - 注意点：
            + 1.0 这里默认安装的3.0版本
            + 2.0 判断安装成功：vue --version/vue -V
    - 2.0 安装：npm install -g @vue/cli-init
        - 注意点：
            + 1.0 如果是3.0版本这个可以完全不用安装
            + 2.0 由于我们要使用2.0版本的生成方式，所以需要下载这个
    - 3.0 生成项目结构
        - vue init webpack 项目名
            - Project name：项目名
            - Project description: 项目描述
            - Author: 作者
            - Vue build：
                - 第一种：配合大部分的开发人员
                - 第二种：仅仅中有runtime
            - Install vue-router? 是否安装vue-router
            - Use ESLint to lint your code?是否使用ESLint来验证我们的语法。
            - Pick an ESLint preser:使用哪种语法规范来检查我们的代码：
                - Standard: 标准规范
                - Airbnb: 爱彼迎规范
            - Set up unit test: 设置单元测试
            - Setup e2e tests： 设置端对端测试
            - Should we run 'npm install':要不要帮忙你下载这个项目需要的第三方包
                - 使用npm来下载
                - 使用yarn来下载
    - 4.0 项目结构介绍
    ```
    ├── build				webpack打包相关配置文件目录
    ├── config				webpack打包相关配置文件目录
    ├── node_modules		 第三方包
    ├── src					项目源码(主战场)
    │   ├── assets			 存储静态资源，例如 css、img、fonts
    │   ├── components		 存储所有公共组件
    │   ├── router			 路由
    │   ├── App.vue			 单页面应用程序的根组件
    │   └── main.js			 程序入口，负责把根组件替换到根节点
    ├── static				可以放一些静态资源
    │   └── .gitkeep		 git提交的时候空文件夹不会提交，这个文件可以让空文件夹可以提交
    ├── .babelrc			 配置文件，es6转es5配置文件，给 babel 编译器用的
    ├── .editorconfig		 给编辑器看的
    ├── .eslintignore	      给eslint代码风格校验工具使用的，用来配置忽略代码风格校验的文件或是目录
    ├── .eslintrc.js		 给eslint代码风格校验工具使用的，用来配置代码风格校验规则
    ├── .gitignore			 给git使用的，用来配置忽略上传的文件
    ├── index.html			 单页面应用程序的单页
    ├── package.json		 项目说明，用来保存依赖项等信息
    ├── package-lock.json	  锁定第三方包的版本，以及保存包的下载地址
    ├── .postcssrc.js		  给postcss用的，postcss类似于 less、sass 预处理器
    └── README.md			 项目说明文档
    ```
+ Standard 规范的细则
