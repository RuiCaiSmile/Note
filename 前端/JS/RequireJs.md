# RequireJs笔记
*少一个requirejs的function参数*

## 简介
RequireJs是一款较旧的前端模块化开发框架，遵循AMD规范，但依旧值得学习。

之所以要模块化开发，是因为研发工程师可以把一个文件重构分割到一个个小文件中，让代码模块化运行，同时能轻松管理各模块之间的关系，耦合性低，可维护性高。研发人员有更多的时间开发功能，提高开发效率。而使用RequireJs也可以解决全局变量污染的问题。

而AMD规范（异步模块定义规范）是RequireJs自己定义的规范，推崇依赖前置。与之相应的还有CMD规范（通用模块加载规范），推崇依赖就近，SeaJs就是例子。

## API
引用RequireJs文件后，会定义三个方法define、require、requirejs，一般使用require。
Define定义一个模块。
Require方法加载并执行模块。

##实例
```
//加载Requirejs,data-main是接口JS文件
//<script type="text/javascript" data-main="js/index.js" src="js/common/require.js"></script>

//config用来配置通用的JS，CSS模块，一般单独放在requirejsConfig.js中
require.config({
	// 基础路径，下面所有的路径都可以省略左边js/部分
	baseUrl : "js/",
	// 配置模块的加载位置和别名
	paths:{
		//这里的css是扩展包的文件名css.js不是css文件夹，用以支持css模块化加载
		loadCSS : "common/css",
		jquery:"common/jquery-1.12.0",
		util : "common/util",
		ddl : "common/DropDownList"
	},
	// 依赖关系：表示加载顺序
	shim:{
		jquery:{
			deps : ["loadCSS!../css/common/common.css"]
		},
		ddl:{
			deps : ["loadCSS!../css/common/DropDownList.css"]
		}
	}
})
//封装组件，兼容写法，同时支持使用<script>标签引用也支持使用模块加载器加载
if ( typeof define === "function" && define.amd ) {
	define( "util", [], function() {
		return util;
	} );
}
//封装组件，直接封装写法
//
define(["jquery","util"],function($,util) {
	// 下拉列表组件
	function DropDownList(args) {
		try {
			if (!args.renderTo)
				throw "renderTo元素不存在，请检查ID是否存在";
			if (args.onClick && !$.isFunction(args.onClick))
				throw "onClick属性必须是方法,现在是" + typeof args.onClick;
			if (args.onComplete && !$.isFunction(args.onComplete))
				throw "onComplete属性必须是方法，现在是" + typeof args.onComplete;
			if (args.preloadItem && !$.isArray(args.preloadItem))
				throw "preloadItem属性必须是数组，现在是" + typeof args.preloadItem;
			if (!args.dataSource)
				throw "缺失必要参数";
		} catch (e) {
			alert("下拉列表初始化失败，原因：" + e);
			return;
		}
		this.init(args);
	};
        //    省略代码

      //结束必须return
	return DropDownList;
})

//加载
require(["jquery","menu"],function($,menu){
		     new menu({
					"renderTo":"menupage",
					"dataSource":[{
						"Name":"用户管理",
						"url":"managerment/table/userManager.jsp"
					},{
						"Name":"商品管理",
						"url":"managerment/table/goodsManager.jsp"
					},{
						"Name":"商品类型管理",
						"url":"managerment/table/typeManager.jsp"
					},{
						"Name":"订单管理",
						"url":""
					},{
						"Name":"购物车管理",
						"url":""
					}]
				});
		});
```
