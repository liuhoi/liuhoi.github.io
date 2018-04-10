---
title: mui+vue结合使用
date: 2017-11-15 20:52:40
tags: [mui,vue,JavaScript]
categories: mui.js+vue.js开发
---


## Mui组件通用CSS类名

### 省略


> 当文字内容超过一行或者多行时，溢出文本用省略号代替。

1. 显示单行：.mui-ellipsis
`<p class="mui-ellipsis">我在一行内，当我超出了会。</p>`

2. 显示两行：.mui-ellipsis-2
`<p class="mui-ellipsis-2">我在一行内，当我超出了会。</p>`

3. 若需要设置显示多行，通过设置-webkit-line-clamp属性，如显示三行添加style=”-webkit-line-clamp:3”:
`<p class="mui-ellipsis-2" style="-webkit-line-clamp:3">我在三行内，当我超出了会显示省略号。我在三行内，当我超出了会显示省略号。我在三行内，当我超出了会显示省略号。我在三行内，当我超出了会显示省略号。我在三行内，当我超出了会显示省略号。我在三行内，当我超出了会显示省略号。</p>`


### 布局

> 整体结构如下：

```
<header class="mui-bar mui-bar-nav">
    <a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
    <h1 class="mui-title">标题</h1>
</header>
<footer class="mui-bar mui-bar-footer">
	底部
</footer>
<div class="mui-content">
	主体
</div>
```

> 当底部内容为选项卡时候，我们会将.mui-bar-footer替换为.mui-bar-tab：

```
<nav class="mui-bar mui-bar-tab">
    <a class="mui-tab-item mui-active">
        <span class="mui-icon mui-icon-home"></span>
        <span class="mui-tab-label">首页</span>
    </a>
    <a class="mui-tab-item">
        <span class="mui-icon mui-icon-phone"></span>
        <span class="mui-tab-label">电话</span>
    </a>
    <a class="mui-tab-item">
        <span class="mui-icon mui-icon-email"></span>
        <span class="mui-tab-label">邮件</span>
    </a>
    <a class="mui-tab-item">
        <span class="mui-icon mui-icon-gear"></span>
        <span class="mui-tab-label">设置</span>
    </a>
</nav>
```

### 局部元素布局

#### 设置边距  (默认为10px外边距)。

`.mui-content-padded`

#### 	设置元素为内联块，内联，块对象

`.mui-inlineblock  .mui-inline   .mui-block`

#### 	浮动

`.mui-clearfix  .mui-pull-left   .mui-pull-right`

#### 	OS环境判断多平台适配：

> mui会通过.mui.os.*方法判断环境，将.mui-plus，.mui-plus-stream，.mui-ios，.mui-android，.mui-wechat，.mui-ios-version，.mui-android-version，.mui-wechat-version绑定在document.body.classList中，我们可以通过这些样式类判断当前的运行判断，于是可以做出一些适配，如：

```
<div class="mui-input-row mui-plus-visible">
    <label>mui-plus-visible</label>
    <input type="text" class="mui-input-speech mui-input-clear" placeholder="我在web环境下隐藏5+环境下显示">
</div>
<div class="mui-input-row mui-plus-hidden">
    <label>mui-plus-hidden</label>
    <input type="text" class="mui-input-clear" placeholder="我在web环境下显示5+环境下隐藏">
</div>
```

> mui中默认在plus环境下和微信环境下设置了样式几个样式：

1. mui-plus-visible：在plus环境下显示，非plsu环境下隐藏
2. mui-wechat-visible：在wechat环境下显示，非wechat环境下隐藏
3. mui-plus-hidden：在plus环境下隐藏，非plsu环境下显示
4. mui-wechat-hidden：在wechat环境下隐藏，非wechat环境下显示

#### 	栅格系统布局

`.mui-row  mui-col-xs-4 mui-col-sm-3`

#### 	区域滚动布局(需手动设置高度，否者滚动区域没有高度)

```
<div class="mui-scroll-wrapper">
	<div class="mui-scroll">
		<!--这里放置真实显示的DOM内容-->
	</div>
</div>
```

> // 常用配置项

```
var options = {
	scrollY: true, //是否竖向滚动
	scrollX: false, //是否横向滚动
	startX: 0, //初始化时滚动至x
	startY: 0, //初始化时滚动至y
	indicators: true, //是否显示滚动条
	deceleration:0.0006 //阻尼系数,系数越小滑动越灵敏
	bounce: true, //是否启用回弹
}
```

> eg：

```
mui('.mui-scroll-wrapper').scroll({
	deceleration: 0.0005 //flick 减速系数，系数越大，滚动速度越慢，滚动距离越小，默认值0.0006
});
```

> 手动设置高度：
> 一般在scroll控件最外面设置div为绝对定位，设置top,bottom,left,right值，为了方便，mui默认设置有个全屏类.mui-fullscreen。

```
.mui-fullscreen {
	position: absolute;
	top: 0;
	right: 0;
	bottom: 0;
	left: 0;
}
```

## Mui常用配置

### 	mui.openWindow配置（最好是通过URL传参数，使用extras有兼容性问题）

```
mui.openWindow({
	url: name + '.html?id=' + id,
	id: name + '.html?id=' + id,
	styles: {
		popGesture: 'close',//苹果返回
	},
	extras: {
		//自定义扩展参数，可以用来处理页面间传值,仅仅首次创建有自定义扩展参数
	},
	createNew: false, //是否重复创建同样id的webview，默认为false:不重复创建，直接显示
	show: {
		autoShow: true, //页面loaded事件发生后自动显示，默认为true
		aniShow: 'slide-in-right', //页面显示动画，默认为”slide-in-right“；
		duration: 200, //页面动画持续时间，Android平台默认100毫秒，iOS平台默认200毫秒；
		event: 'titleUpdate', //页面显示时机，默认为titleUpdate事件时显示
		extras: {} //窗口动画是否使用图片加速
				},
		waiting: {
		autoShow: true, //自动显示等待框，默认为true
		title: '加载中', //等待对话框上显示的提示内容
		options: {
						//width:120,等待框背景区域宽度，默认根据内容自动计算合适宽度
						//height:100,等待框背景区域高度，默认根据内容自动计算合适高度
		}
	}
})
```

### 单webview上拉加载下拉刷新配置

```
mui.init({
	swipeBack: false,
	pullRefresh: {
		container: '#pullrefresh',
		down: {
			style: 'circle',
			offset: '44px',
			contentdown: "下拉可以刷新", //可选，在下拉可刷新状态时，下拉刷新控件上显示的标题内容
		    contentover: "释放立即刷新", //可选，在释放可刷新状态时，下拉刷新控件上显示的标题内容
			contentrefresh: "正在刷新...", //可选，正在刷新状态时，下拉刷新控件上显示的标题内容
			auto: false,
			callback: function(){}
		},
		up: {
			auto: false,
			contentrefresh: '正在加载...',
			callback: function(){}

		}
	}
});
```

### 下拉刷新和上拉加载实现(下拉刷新中有重置上拉加载的方法)

> 下拉刷新

```
function() { //下拉刷新具体业务实现				
	setTimeout(function() {	
		index = 1;
		alert(' 下拉刷新');			
		mui('#pullrefresh').pullRefresh().refresh(true)			
		mui('#pullrefresh').pullRefresh().endPullupToRefresh(false); //参数为true代表没有更多数据了。		
		mui('#pullrefresh').pullRefresh().endPulldownToRefresh(); //refresh completed
	}, 500);
}
```

> 上拉加载

```	
function() { //上拉加载具体业务实现
	setTimeout(function() {
		index++
		mui('#pullrefresh').pullRefresh().endPullupToRefresh(index>pageAll); //参数为true代表没有更多数据了。
		if(index<=pageAll){
			alert(' 上拉加载');
		}
	}, 500);
}
```

### Ajax请求

```
mui.ajax({
	url: url,//请求地址
	data: data,//请求参数
	async: true,
	dataType: 'json',
	crossDomain: true, //强制使用5+跨域
	type: 'post',
	timeout: 28000,
	headers: {'Content-Type': 'application/x-www-form-urlencoded},
	beforeSend: function(){},
	success: function(){},
	error: function(XMLHttpRequest, textStatus, errorThrown) {},
	complete: function(){}
});
```

### Mui与jq冲突解决办法

`var $j = jQuery.noConflict(true);`

### Mui传值

> 传递参数

```
mui.fire(webview, 'show', {
	id: 'id', //传往对应webview的值  
	name: 'name2' //传往对应webview的值  
});
```

> 	接受参数
	
```
window.addEventListener('show', function(event) {
	//获得事件参数  
	var id = event.detail.id;
	var name = event.detail.name;
});
```

### Div模式下顶部渐变配置

```
<div class="mui-scroll-wrapper">
	<div class="mui-scroll">
		<!--这里放置真实显示的DOM内容-->
	</div>
</div>
<header class="mui-bar mui-bar-transparent">
	<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
	<h1 class="mui-title">产品信息</h1>
</header>
<div class="mui-content">
	<!--背景图片-->
	<img id="img1" src="../../img/homePage/detailBg.png">
</div>
```

> 常用配置项
> 顶部渐变参数

```
mui('.mui-bar-transparent').transparent({
	top: 0,
	offset: 54,
	duration: 16,
	scrollby: document.querySelector('.mui-scroll-wrapper') || window
})
```

### Mui返回时刷新

```
	var wobj = plus.webview.getWebviewById(opener);
	wobj.reload(true);
```

### Mui中判断是否断网

```
if(window.plus && plus.networkinfo.getCurrentType() === plus.networkinfo.CONNECTION_NONE) {
	plus.nativeUI.toast('似乎已断开与互联网的连接', {
		verticalAlign: 'top'
	});
	return;
}
```

## mui常用函数封装

### 常用Picker

> poppicker组件依赖mui.picker.js/.css mui.poppicker.js/.css请务必在mui.js/css后中引用,也可统一引用 压缩版本:mui.picker.min.js
> 备注：可通过 instance.pickers[index] 拿到指定层级的实例，然后通过setSelectedIndex()和setSelectedValue()两个方法,设定指定层级的选中项

```
var picker = new mui.PopPicker();
picker.setData([{
    value: "first",
    text: "第一项"
}, {
    value: "second",
    text: "第一项"
}, {
    value: "third",
    text: "第三项"
}, {
    value: "fourth",
    text: "第四项"
}, {
    value: "fifth",
    text: "第五项"
}])
//picker.pickers[0].setSelectedIndex(4, 2000);
picker.pickers[0].setSelectedValue('fourth', 2000);
picker.show(function(SelectedItem) {
	picker.dispose();
})
```

### 地址选择器

> poppicker组件依赖mui.picker.js/.css mui.poppicker.js/.css请务必在mui.js/css后中引用,也可统一引用 压缩版本:mui.picker.min.js

```
var picker = new mui.PopPicker({
    layer: 2//设置几级联动
});
picker.setData([{
    value: '110000',
    text: '北京市',
    children: [{
            value: "110101",
            text: "东城区"
    }]
}, {
    value: '120000',
    text: '天津市',
    children: [{
        value: "120101",
        text: "和平区"
    }, {
        value: "120102",
        text: "河东区"
    }, {
        value: "120104",
        text: "南开区"
    }
    ]
}])
picker.pickers[0].setSelectedIndex(1);
picker.pickers[1].setSelectedIndex(1);
picker.show(function(SelectedItem) {
	picker.dispose()；
})
```

### 时间选择器

> poppicker组件依赖mui.picker.js/.css mui.poppicker.js/.css请务必在mui.js/css后中引用,也可统一引用 压缩版本:mui.picker.min.js

```
var dtPicker = new mui.DtPicker(); 
var options = {
	type: "date", //设置日历初始视图模式  详情参考mui
	beginDate: new Date(year, month, day), //设置开始日期 
};
dtPicker.show(function (rs) { 
   /*
	* rs.value 拼合后的 value
	* rs.text 拼合后的 text
	* rs.y 年，可以通过 rs.y.vaue 和 rs.y.text 获取值和文本
	* rs.m 月，用法同年
	* rs.d 日，用法同年
	* rs.h 时，用法同年
	* rs.i 分（minutes 的第二个字母），用法同年
	*/
})
```

### actionSheet 封装

```
function(){		
	var btnArray = [{title:"选取现有的"}];
	plus.nativeUI.actionSheet( {
		title:"选择照片",
		cancel:"取消",
		buttons:btnArray
	}, function(e){
		var index = e.index;
		switch (index){
			case 0:
				//取消
				function(){}
				break;
			case 1:
				//选择照片
				function(){}
				break;
		}
	} );
}	
```

### 选取照片

``` 
function(){//获取图片元素
	plus.gallery.pick(function(path) {
		var ele = document.getElementById("imgUp");
		ele.innerHTML = '';
		var img = new Image();
		img.src = path;
		ele.appendChild(img);	            
		upload();
	}, function() {}, {
		filter: "image"
	});
}

function upload(){// 上传文件
	var files = document.querySelector('#imgUp');
	var wt=plus.nativeUI.showWaiting();
	var server = "http://192.168.2.01:3000/xxx/UploadFile";
    var task=plus.uploader.createUpload(server,{method:"POST"},function(t,status){ //上传完成
        if(status==200){
        	var data = {};
        	data.photo = t.responseText.split(',')[0]+t.responseText.split(',')[1];
            console.log("上传成功："+t.responseText);
            	Ajax.then(function(r){//请求})
            wt.close(); //关闭等待提示按钮
        }else{
            console.log("上传失败："+status);
            wt.close();//关闭等待提示按钮
        }
    });
    //添加其他参数
    task.addData("name","test");
    task.addFile(files.src,{key:"dddd"});
    task.start();				
}			
```

## Mui问题BUG解决办法

### IOS端键盘弹出后顶部导航栏乱飞

> HTML结构

```
<div class="mui-scroll-wrapper mui-content">
	<div class="mui-scroll">
		<!--这里放置真实显示的DOM内容-->
	</div>
</div>
```

> JS

```
mui('.mui-scroll-wrapper').scroll({
	deceleration: 0.0005 //flick 减速系数，系数越大，滚动速度越慢，滚动距离越小，默认值0.0006
});
if(window.plus && mui.os.ios){
	var ws = plus.webview.currentWebview();
	ws.setStyle({
		softinputMode:'adjustResize'
	});
}
```

### IOS端由BUG1引起的光标错位

>  修改mui.js如下添加判断  ios端关闭'translate3d

```
_getTranslateStr: function(x, y) {
	if (this.options.hardwareAccelerated && mui.os.android) {
	return 'translate3d(' + x + 'px,' + y + 'px,0px) ' + this.translateZ;
	}
	return 'translate(' + x + 'px,' + y + 'px) ';
}
```

## Mui结合vue的方案

### 在vue中使用picker封装(单个picker)（结合BUG1， BUG2方案使用）

> 备注：需在mui.js中picker取消和蒙版消失处添加setTimeout(function(){a.panel.style.display="none"},350)  解决键盘弹出后会重新计算webview导致picker出现的问题

>  HTML结构

`<input type="text" v-picker="{name:'picker'}" readonly >`

>  JS

```
mounted: function() {
	$j('.mui-poppicker').hide();//解决键盘弹出后会重新计算webview导致picker出现的问题
},
directives:{
	picker:function(el, binding){
		binding.value.name = new mui.PopPicker();			
		binding.value.name.setData(binding.value.data);
		el.addEventListener('tap', function(event) {	
			$j(binding.value.name.panel).show();
			document.activeElement.blur();//失去焦点		
			binding.value.name.show(function(selectItems) {
				$j(el).val(selectItems[0].text);
				setTimeout(function(){
					$j(binding.value.name.panel).hide();
				},350)
			binding.value.name.hide();
			/*binding.value.name.dispose();只能选一次*/
			});
		}, false);
	}
}

```

### 在vue中使用picker封装(多个在1的基础上修改)

```
Ajax.then(function(r){
	localStorage.relatives_type = JSON.stringify(app.relatives_type)
	localStorage.edu_status = JSON.stringify(app.edu_status)
})
	
directives:{
	picker: {
		bind:function(el, binding){
			var data=binding.value.pickerType == 0 ? JSON.parse(localStorage.edu_status): JSON.parse(localStorage.relatives_type);
			binding.value.name = new mui.PopPicker();			
			binding.value.name.setData(data);
			el.addEventListener('tap', function(event) {	
				$j(binding.value.name.panel).show();
				document.activeElement.blur();//失去焦点		
				binding.value.name.show(function(selectItems) {
					$j(el).val(selectItems[0].text);
					setTimeout(function(){
						$j(binding.value.name.panel).hide();
					},350)
				binding.value.name.hide();
				/*binding.value.name.dispose();只能选一次*/
				});
			}, false);
		}
	}
}
```

### 在vue中使用:style

`:style="{backgroundImage: 'url('+c.imageUrl+')'}"`	

### 在vue做版本更新

#### 本地JS

> * 配置
> * 不能在vue里面创建  只能在外面创建

```
var server="http://www.xrtxdgs.com/XRT/static/version/version.json",//获取升级描述文件服务器地址
localDir="update",localFile="version.json",//本地保存升级描述目录和文件名
keyUpdate="updateCheck",//取消升级键名
keyAbort="updateAbort",//忽略版本键名
checkInterval=604800000,//升级检查间隔，单位为ms,7天为7*24*60*60*1000=604800000, 如果每次启动需要检查设置值为0
dir=null;
```

> * 准备升级操作
> * 创建升级文件保存目录

```
initUpdate:function (){// 在流应用模式下不需要检测升级操作	
	if(navigator.userAgent.indexOf('StreamApp')>=0){
		return;
	}
	// 打开doc根目录
	plus.io.requestFileSystem( plus.io.PRIVATE_DOC, function(fs){
		fs.root.getDirectory(localDir, {create:true},function(entry){	
			dir = entry;
			app.checkUpdate();
			console.log( "当前现在目录    "+entry.name );
		}, function(e){
			console.log( "准备升级操作，打开update目录失败："+e.message );
		});
	},function(e){
		console.log( "准备升级操作，打开doc目录失败："+e.message );
	});
},
```

> * 检测程序升级

```
checkUpdate:function() {
	// 判断升级检测是否过期
	/*var lastcheck = plus.storage.getItem(keyUpdate );
	if ( lastcheck ) {
		var dc = parseInt( lastcheck );
		var dn = (new Date()).getTime();
		if ( dn-dc <checkInterval ) {	// 未超过上次升级检测间隔，不需要进行升级检查
			return;
		}
		// 取消已过期，删除取消标记
		plus.storage.removeItem(keyUpdate );
	}*/
	// 读取本地升级文件
	dir.getFile(localFile, {create:false}, function(fentry){
		fentry.file( function(file){
			var reader = new plus.io.FileReader();
			reader.onloadend = function ( e ) {
				/*fentry.remove();*/
				app.getUpdateData();
				var data = null;
				try{
					data=JSON.parse(e.target.result);
				}catch(e){
					console.log( "读取本地升级文件，数据格式错误！" );
					return;
				}
				app.checkUpdateData( data );
			}
			reader.readAsText(file);
		}, function(e){
			console.log( "读取本地升级文件，获取文件对象失败："+e.message );
			fentry.remove();
		} );
	}, function(e){
		// 失败表示文件不存在，从服务器获取升级数据
		console.log( "本地升级文件不存在,从服务器获取升级数据");
		app.getUpdateData();
	});
},
```

> * 检查升级数据

```
checkUpdateData:function( j ){
	var curVer=plus.runtime.version;
	console.log(curVer)
	inf = j[plus.os.name];
	console.log(JSON.stringify(j[plus.os.name]))
	if ( inf ){
		var srvVer = inf.version;
		// 判断是否存在忽略版本号
		var vabort = plus.storage.getItem( keyAbort );
		if ( vabort && srvVer==vabort ) {
			// 忽略此版本
			return;
		}
		// 判断是否需要升级
		if ( this.compareVersion(curVer,srvVer) ) {
			// 提示用户是否升级
			plus.nativeUI.confirm( inf.note, function(i){
				if ( 0==i.index ) {
					plus.runtime.openURL( inf.url );
				} else if ( 1==i.index ) {
					plus.storage.setItem( keyAbort, srvVer );
					plus.storage.setItem( keyUpdate, (new Date()).getTime().toString() );
				} else {
					plus.storage.setItem( keyUpdate, (new Date()).getTime().toString() );
				}
			}, inf.title, ["立即更新","跳过此版本","取　　消"] );
		}
	}
},
```

> * 从服务器获取升级数据

```
getUpdateData:function (){
	var xhr = new plus.net.XMLHttpRequest();
	xhr.onreadystatechange = function () {
        switch ( xhr.readyState ) {
            case 4:
                if ( xhr.status == 200 ) {
                	// 保存到本地文件中
                	dir.getFile( localFile, {create:true}, function(fentry){
                		fentry.createWriter( function(writer){
                			writer.onerror = function(){
                				console.log( "获取升级数据，保存文件失败！" );
                			}
                			writer.write( xhr.responseText );
                		}, function(e){
                			console.log( "获取升级数据，创建写文件对象失败："+e.message );
                		} );
                	}, function(e){
                		console.log( "获取升级数据，打开保存文件失败："+e.message );
                	});
                } else {
                	console.log( "获取升级数据，联网请求失败："+xhr.status );
                }
                break;
            default :
                break;
        }
	}
	xhr.open( "GET", server );
	xhr.send();
},
```

> * 比较版本大小，如果新版本nv大于旧版本ov则返回true，否则返回false
> * @param {String} ov
> * @param {String} nv
> * @return {Boolean} 

```
compareVersion:function ( ov, nv ){
	if ( !ov || !nv || ov=="" || nv=="" ){
		return false;
	}
	var b=false,
	ova = ov.split(".",4),
	nva = nv.split(".",4);
	for ( var i=0; i<ova.length&&i<nva.length; i++ ) {
		var so=ova[i],no=parseInt(so),sn=nva[i],nn=parseInt(sn);
		if ( nn>no || sn.length>so.length  ) {
			return true;
		} else if ( nn<no ) {
			mui.toast('暂无更新')
			return false;
		}
	}
	if ( nva.length>ova.length && 0==nv.indexOf(ov) ) {
		return true;
	}
}
```
	
#### 服务器上

	
`var server = serverUrl +"UpdateBsnLicense";//json服务器路径`

> * 服务器上面的version.json

```
{
    "appid": "App",
    "iOS": {
        "version": "1.0.1",
        "title": "1.0.1版本更新",
        "note": "新增自动升级检测功能\n新增分享功能演示页面\n新增推送功能演示页面\n",
        "url": "itms-apps://itunes.apple.com/cn/app/hello-h5+/id682211190?l=zh&mt=8"
    },
    "Android": {
        "version": "1.0.1",
        "title": "1.0.1版本更新",
        "note": "新增自动升级检测功能\n新增分享功能演示页面\n新增推送功能演示页面\n",
        "url": "http://www.app.com/app/static/version/app.apk"
    }
}
```

### 在vue中循环中倒计时

> HTML

`<div v-time-countdown="{getTime:getTimeCoutDown,time:'2017-10-10'}"></div>`

> JS

```
getTimeCoutDown:function(time){
	var leftTime = time;
	if(leftTime<=0){
		return (0 + "天" + 0 +"小时" + 0 +"分" + 0 +"秒");
	else{
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
		return ("<b>倒计时：</b><span>"+days+"</span>日<span>"+hours+"</span>小时<span>"+minutes+"</span>分");
	}
}
directives:{
	'time-countdown':{
		bind:function(el, binding){
			el.innerHTML = binding.value.getTime(new Date(binding.value.time) - new Date())	
			setInterval(function(){
				now = new Date();
				el.innerHTML = binding.value.getTime(new Date(binding.value.time) - now)	;
			},1000);
		}
	}
}
```


### 在vue中使用上传图片

> CSS

> HTML



> JS

```
new Vue({
	el:'#app',
	data:{
		sendInfo:{
			imgUrl:''
		},
		nativeUrl:''
	},
	methods:{
		upload:function(){//获取图片元素
			plus.gallery.pick(function(path) {     
				upload(path);
			}, function() {}, {
				filter: "image"
			});
		},
		upload:function(path){// 上传文件
			var wt=plus.nativeUI.showWaiting();
			var server = "http://192.168.2.01:3000/xxx/UploadFile";
		    var task=plus.uploader.createUpload(server,{method:"POST"},function(t,status){ //上传完成
		        if(status==200){
		        	var data = {};
		        	data.photo = t.responseText.split(',')[0]+t.responseText.split(',')[1];
		            console.log("上传成功："+t.responseText);
		            	Ajax.then(function(r){//请求})
		            wt.close(); //关闭等待提示按钮
		        }else{
		            console.log("上传失败："+status);
		            wt.close();//关闭等待提示按钮
		        }
		    });
		    //添加其他参数
		    task.addData("name","test");
		    task.addFile(files.src,{key:"dddd"});
		    task.start();				
		}		
	}
	
})
```


	