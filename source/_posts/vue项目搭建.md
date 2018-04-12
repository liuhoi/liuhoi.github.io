---
title: vue项目搭建
date: 2018-04-12 20:17:46
tags: [vue]
categories: vue
---



## 第一部分：vue项目的搭建

### vue项目的搭建

全局安装 vue-cli

`
$ npm install --global vue-cli
`

创建一个基于 webpack 模板的新项目

`
$ vue init webpack my-project
`

安装依赖，走你

```
$ cd my-project
$ npm install
$ npm run dev
```


## 第二部分:插件

### 1.	Sass的引进

#### 1.	npm命令

```
$ npm install node-sass -–save-dev
$ npm install sass-loader –-save-dev
```

#### 2.	配置webpack.base.conf.js 

```
module.exports={
	module:{
		rules:[
			{
		        test: /\.scss$/,
		        loaders: ["style", "css", "sass"]
	       	},	
		]
	}
}
```

#### 3.	Xxx.vue中使用

```
<style  lang="scss" scoped type="text/css"></style>
```

### 2.	jquery的引进

#### 1.	npm命令

$ npm install –-save jquery 
#### 2.	配置webpack.base.conf.js 

在开头引入webpack，后面的plugins那里需要

`
var webpack = require('webpack')
`
```
module.exports = {
   // 其他代码...
   resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
      'jquery': 'jquery' 
    }
  },
   // 增加一个plugins
   plugins: [
      new webpack.ProvidePlugin({
          $: "jquery",
          jQuery: "jquery"
      })
   ],

   // 其他代码...
}
```

### 3.	layui的引进(不建议使用)

直接script,link方式引用 

```
<link rel="stylesheet" type="text/css" href="static/lib/layui/css/layui.css"/>
<link rel="stylesheet" type="text/css" href="static/lib/swiper-2.7.6/idangerous.swiper.css">
<link rel="stylesheet" type="text/css" href="static/lib/swiper-2.7.6/idangerous.swiper.scrollbar.css">
```

```
<script src="static/lib/layui/layui.js"></script>
<script src="static/lib/swiper-2.7.6/idangerous.swiper.min.js"></script>
<script src="static/lib/swiper-2.7.6/idangerous.swiper.scrollbar-2.1.js"></script>
```

### 4.	element-ui的引进(建议使用)

#### 1.	npm命令

`
npm i element-ui --save
`

#### 2.	配置main.js 

在main.js中写入一下内容

```
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
Vue.use(ElementUI)
```

### 5.	axios的引进(ie9不支持需引入polyfill)

暂时采用挂载在Vue原型上面

#### 1.	npm命令

```
$ npm install -–save axios
$ npm install es6-promise --save-dev
```

#### 2.	在ajax.js 

```
import qs from 'qs'
import axios from 'axios'
import promise from 'es6-promise'
promise.polyfill();

//请求地址
axios.defaults.baseURL = 'http://www.baid.com/';

//请求超时时间。
axios.defaults.timeout = 10000;
//添加一个请求拦截器
axios.interceptors.request.use(function(config){
    //在请求发送之前做一些事
  	config.url = config.url+'.do';
  	config.headers = {
  		'Content-Type':'application/x-www-form-urlencoded;charset=utf-8'
  	};
    return config;
},function(error){
    //当出现请求错误是做一些事
    return Promise.reject(error);
});
//请求时对数据进行转化。
axios.defaults.transformRequest = [function (data, headers) {
  if(headers['Content-Type']==='application/json'){
  	return data;
  
  }else{
  	return qs.stringify(data);
  }
}];
export default (axios);
```

#### 3.	在main.js 

```
import axiosInstance from '@/assets/ajax'

Vue.prototype.$axios = axiosInstance;
```

#### 4.	在xxx.VUE中使用

```
methods:{
	guideDetailFn:function(){//服务指南详情
		this.$axios({
			method: 'post',
			url: 'serviceGuide/getServiceGuide',
			params: {
				id:this.$route.params.id,
			},
		}).then(function (response) {
	        		
		}.bind(this))
		.catch(function (error) {
			 console.log(error);
		});
	},
},
created:function(){
    this.$axios.all([this.guideDetailFn()])
	.then(this.$axios.spread(function (acct, perms) {
	// Both requests are now complete
	}));
 },
```

### 6.	Es6兼容ie9

#### 1.	npm命令

`
$ npm install babel-polyfill -–save-dev
`

#### 2.	在webpack.base.conf.js 

```
module.exports = {
  entry: {
    'babel-polyfill': 'babel-polyfill',
    app: './src/main.js'
  }
}
```

### 7.	Vuex引用
#### 1.	npm命令

`
$ npm install vuex –-save
`

#### 2.	在main.js中配置

```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  } })
new Vue({
  el: '#app',
  router,
  store,
  template: '<App/>',
  components: { App }
})
```

### 8.	Vue-lazyload引用

#### 1.	npm命令

`
$ npm install vue-lazyload --save
`

#### 2.	在main.js中配置

```
import VueLazyload from 'vue-lazyload'

Vue.use(VueLazyload, {
  preLoad: 1.3,
  error: 'dist/error.png',
  loading: 'dist/loading.gif',
  attempt: 1
})
```
### 9.	图片路径问题

#### 1.	在config文件夹中index.js中配置

```
build: {
    env: require('./prod.env'),
    index: path.resolve(__dirname, '../dist/index.html'),
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    productionSourceMap: true,
    // Gzip off by default as many popular static hosts such as
    // Surge or Netlify already gzip all static assets for you.
    // Before setting to `true`, make sure to:
    // npm install --save-dev compression-webpack-plugin
    productionGzip: false,
    productionGzipExtensions: ['js', 'css'],
    // Run the build command with an extra argument to
    // View the bundle analyzer report after build finishes:
    // `npm run build --report`
    // Set to `true` or `false` to always turn it on or off
    bundleAnalyzerReport: process.env.npm_config_report
  },
改为
build: {
    env: require('./prod.env'),
    index: path.resolve(__dirname, '../dist/index.html'),
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: './',
    productionSourceMap: true,
    // Gzip off by default as many popular static hosts such as
    // Surge or Netlify already gzip all static assets for you.
    // Before setting to `true`, make sure to:
    // npm install --save-dev compression-webpack-plugin
    productionGzip: false,
    productionGzipExtensions: ['js', 'css'],
    // Run the build command with an extra argument to
    // View the bundle analyzer report after build finishes:
    // `npm run build --report`
    // Set to `true` or `false` to always turn it on or off
    bundleAnalyzerReport: process.env.npm_config_report
  },
```

#### 2.	在build文件夹中utils.js中配置

```
if (options.extract) {
      return ExtractTextPlugin.extract({
        use: loaders,
        fallback: 'vue-style-loader'
      })
    } else {
      return ['vue-style-loader'].concat(loaders)
    }
改为
if (options.extract) {
  	return ExtractTextPlugin.extract({
    	use: loaders,
    	publicPath:'../../',
    	fallback: 'vue-style-loader'
  	})
} else {
  	return ['vue-style-loader'].concat(loaders)
}
```


### 10.	路径只是参数变化

如果目的地和当前路由相同，只有参数发生了改变 (比如从一个用户资料到另一个 /users/1 -> /users/2)，使用 beforeRouteUpdate 来响应这个变化

#### 1.	在xxx.VUE中使用

```
beforeRouteUpdate:function(to, from, next) {
	next();
	this.axjax();//查询请求
},
methods:{
	nextDetail:function(){
		this.$router.push({name:this.$route.name,params:{id:this.detailNext.id}})	
	}
}
```

注意：使用name不使用path使用path会出现错误

### 11.	Chrome浏览器安装vue-devtools（先按照官网的教程安装  如报错在使用下面的方法）

```
下载devtools
打开下载好的vue-devtools文件夹
打开package.json文件 
找到依赖包devDependencies里面的以下三个包进行删除 buble,buble-loader,selenium-server
进入文件夹vue-devtools打开命令窗口
下载以上删除的包：
cnpm install buble
cnpm install buble-loader
cnpm install wbuf 
cnpm install selenium-server(该安装过程可能会卡在那里，按ctrl+c结束,不影响) 
下载依赖包里的安装包 cnpm install 
运行 npm run build
进入文件夹vue-devtools 

进入shells-->chrome-->manifest.json打开修改以下内容：
找到以下部分：
	"persistent":false 
	修改为：
 	"persistent":true 
进入shells-->chrome-->webpack.config.js打开修改以下内容：(按照自己的需求来)
 找到以下部分：（最后面）
 	'process.env':{NODE_ENV:"production"}
 	修改为： 
	'process.env':{NODE_ENV:"development"}
把插件应用到浏览器
找到文件夹vue-devtools\shells下的chrome文件夹 
直接拖进浏览器的更多工具-->扩展程序里
```

### 12.	上传到服务器
#### 1.	npm命令

`
$ npm run build
`
```
命令执行后会在当前工程在创建dist文件夹
将dist目录放在服务器中就可以了
```

## 第三部分:自定义组件

### 1.	倒计时组件

#### 1.	Html

`
<code><count-down :time="info.biddingTime"></count-down></code>
`

#### 2.	js

```
Vue.component:{
	'count-down':{
		template:'<span class="time">{{needTime}}</span>',
		data:function(){
			return{
				needTime:''
			}
		},
		props:['time'],
		methods:{
			dealTime:function(time){	
				var leftTime = time;
				var days = parseInt(leftTime / 1000 / 60 / 60 / 24 , 10); //计算剩余的天数 
				var hours = parseInt(leftTime / 1000 / 60 / 60 % 24 , 10); //计算剩余的小时 
				var minutes = parseInt(leftTime / 1000 / 60 % 60, 10);//计算剩余的分钟 
				var seconds = parseInt(leftTime / 1000 % 60, 10);//计算剩余的秒数 
				days = checkTime(days); 
				hours = checkTime(hours); 
				minutes = checkTime(minutes); 
				seconds = checkTime(seconds); 
				function checkTime(i){ //将0-9的数字前面加上0，例1变为01 
				 if(i<10) 
				 { 
				  i = "0" + i; 
				 } 
				 return i; 
				} 
				return (hours+":" + minutes+":"+seconds);
			},
			getTime:function(time){
				var date = time - Date.now();
				if(date<=0){
					this.needTime  = '00:00:00';
				}else{
					this.needTime = this.dealTime(date);
					var interval =  setInterval(function(){
  						date = time - Date.now();
  						if(date<=0){
  							clearInterval(interval);
  							this.needTime = '00:00:00';
  						}else{
  							this.needTime = this.dealTime(date);
  						}
					}.bind(this),1000)
				}
				
			}
		},
		created:function(){
			this.needTime = + new Date(this.time.replace(/\-/g,'/').replace(/\.0/g,''));
			this.getTime(this.needTime);
		}
	}
}
```