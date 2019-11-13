# Vue2.5构建去哪儿网

一个仿去哪儿APP的移动端项目练习，有主页，详情页，城市列表页，组件化开发以及CSS样式预处理框架stylus,通过variable.styl设置样式变量，抽离出公用样式减少代码重复，也便于维护，还使用了vue-awesome-swiper、better-scroll、axios插件。

练习了使用Vue开发中常用的路由管理器Vue-Router来构建单页面应用，状态管理模式Vuex，异步处理方案axios, 使用了LocalStorage实现页面数据持久存储, 使用keep-alive对组件进行缓存，优化路由性能，结合activated生命周期钩子进行ajax请求优化。

## 技术栈
* Vue: Vue、Vue-router、Vuex、Vue-cli
* 插件：axios、vue-swesome-swiper、better-scroll
* css样式预设置：stylus
* api：后台接口数据

## 生成文件的部分说明
&emsp;build文件 -- 项目webpack打包配置内容

&emsp;config文件 -- 项目配置内容

&emsp;node_modules文件 -- 第三方依赖包

&emsp;src文件 --  存放整个项目的源代码

&emsp;&emsp;&emsp;src/router文件 -- 项目的所有路由

&emsp;&emsp;&emsp;src/assets文件 -- 会被webpack打包成依赖模块

&emsp;&emsp;&emsp;src/App.vue -- 项目的根组件

&emsp;&emsp;&emsp;src/main.js -- 项目的入口文件

&emsp;static文件 -- 里面的文件不会被webpack打包处理，而是被直接复制到最终目录（dist/static）下

&emsp;index.html -- 默认页面


## 项目特点
* 组件化开发
* 易于维护

## 项目分为几部分？
* 首页部分
* 详情页部分
* 城市选择部分

## 项目怎样拆分？
* 按照页面拆分，src/pages目录下每个夹代表一个页面
* 页面文件夹（父组件）下管理组成页面的子组件文件，父子组件间利用`props`传值。组件化使每部分的代码更清晰，易于维护
* 在router/index.js中管理路由

# 首页部分

1.  iconfont图标引入与使用 
2. 使用vue-swesome-swiper轮播插件
3. 公用样式封装
4. axios获取接口数据
5. 父子组件间传值

### 使用vue-awesome-swiper轮播组件（2.6.7版本）
安装这个插件

    npm install vue-awesome-swiper --save
    
用import在入口文件（main.js）中导入

    import VueAwesomeSwiper from 'vue-awesome-swiper'
    
为了**防止因为网速过慢而引起的页面抖动**，我们将css样式设置成

    overflow hidden
    width 100%
    height 0
    padding-bottom 31.25%
    
使包裹插件的div的高度由宽度的`27%`撑开。

这里要注意的是，vue-awesome-swiper插件拥有自己的样式，要让它使用我们定义的样式，需要用到>>>和ref

### 构建多页切换
利用计算属性根据图标数量判断当前图标在哪个页面，构建多页切换功能

      computed: {
        pages: function () {
            const res = []
            for (let i in this.iconList) {
                const k = Math.floor(i / 8)
                if (!res[k]) {
                    res[k] = []
                }
                res[k].push(this.iconList[i])
            }
            return res
        }
      }

 
### 样式封装

将公用样式封装在varibles.styl和mixins.styl文件中，有以下几点好处：

&emsp; 1.可以方便得被组件使用，避免重复编写样式

&emsp; 2.便于后期维护

### 使用`axios`进行ajax请求

安装axios插件

将静态文件放在static文件夹下，这些静态文件能被浏览器直接读取

开发环境转发代理，设置配置文件(config/index.js)下`dev`的`proxyTable`内容，webpack-dev-serve工具会自动将`/api`转换成`static/mock`

     proxyTable: {
      '/api': {
        target: 'http://localhost:8081/',
        pathRewrite: {
          '^/api':'/static/mock'
        }
      }
    }

用import在父组件导入axios，发送get请求

     axios.get('/api/index.json')

# 城市选择部分

1. router-link内部组件的使用
2. 使用better-scroll插件


### 实现页面跳转
使用`<router-link>`内部组件通过路由实现跳转页面的功能，`to`后边加需要跳转的页面的路由，（`to = "/"`表示跳转到根路径下的页面）

    <router-link to="/">
      <div class="header-left">
        <div class="iconfont back-icon">&#xe606;</div>
      </div>
    </router-link>
    
### 使用better-scroll插件

首先通过修改样式使**页面固定**，后续搭配better-scroll插件实现原声APP的页面上下滑动的效果

将 HTML DOM 结构调整成文档中规定的结构，在外层取`wrapper`，引用插件之后，在 `mounted ()` 生命周期钩子里面新建一个这个 DOM 引用的实例。

    import Bscroll from 'better-scroll'
    export default {
      name: 'CityList',
      //生命周期函数 挂载之后执行
      mounted () {
        //引用 wrapper DOM
        this.scroll = new Bscroll(this.$refs.wrapper)
      }
    }
    
### alphabet

是一个显示在
