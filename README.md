# 百度MIP扩展组件开发手册（不依赖编译版）

## 开发之前

### 1、代码规范

JS

https://github.com/ecomfe/spec/blob/master/javascript-style-guide.md

CSS

https://github.com/ecomfe/spec/blob/master/css-style-guide.md

需通过规范检查：

http://fecs.baidu.com/

### 2、github 新建 ISSUE，申请自定义 html 标签名


## 怎么开发

	1、在页面头部引入 css：<link rel="stylesheet" type="text/css" href="https://mipcache.bdstatic.com/static/mipmain-v0.0.1.css">

	2、在 body 最后引入 js：<script src="https://mipcache.bdstatic.com/static/mipmain-v0.1.1.js"></script> todo 待上线

	3、在2的 js 后面外链或内联你的组件 js，若页面依赖多个组件，依次引入组件 js，写法参照 组件 js 构成介绍。

### 组件 js 构成介绍(需要按照最新的改一下)

```
define('mip-demoforall/* 你的组件名称，需要更改1 */', ['require', 'customElement'], function(require) {

    var customElem = require('customElement');

    customElem.prototype.init = function() {

        this.build = render;
        /* 按生命周期自定义，生命周期参考组件的生命周期，需要更改2 */

    };

    /* 自定义的 js 功能函数 start 需要更改3 */
    function render() {
        if (this.isRender) {
            return; 
        }
		this.isRender = true;
        /* 前四行保留，防止组件的 render 函数多次调用 */

        console.log('test');
    }
    /*自定义的 js 功能函数 end*/

    return customElem;
});
require(['mip-demoforall/* 你的组件名称，需要更改4 */'], function (demoforall) {
	//若组件需要有 css,自测时先用字符串，提交过来需要使用 __inline('./组件名称.less'),一个 less 文件
	MIP.css.mipDemoforall = ".mip-demo-f13 {font-size: 13px;}";
    //注册组件,若有 css 才加第三个参数，否则不要第三个参数
    MIP.registerMipElement('mip-demoforall/* 你的自定义 html 标签名，一般同组件名，需要更改5 */', demoforall, MIP.css.mipDemoforall/*  css  string，需要更改6 */');
});
```

### 组件的生命周期
	
	待补充


## 怎么测试
	
	本地测试，直接静态的 html 文件用浏览器打开即可

	测试覆盖范围  待补充

## 测试 OK 了之后需要做什么？

1、书写详细的组件功能说明文档
	
	参照：https://www.mipengine.org/doc.html#../docs/4-widget/4-customize-widget/appdl-widget.md

2、代码提交

### 线上github 方式
	
```

第一步，建分支，删本地
 git checkout -b myfeature;
 git push origin myfeature;
 git checkout master;
 git branch -d myfeature;

第二步，拉分支，做开发
 git checkout -b dperf origin/dperf;
 git add xxx; git commit -m 'dev xxx';
 git push origin dperf;

代码push到origin服务器后，可以去网页上发起Merge Request

第三步, 合并分支
 git checkout master;
 git pull;
 git checkout dperf;
 git fetch origin master;
 git rebase master;

 ```

 ### 线下方式 qq
	
	1、申请 MIP qq 号值周同学进行代码的 codereview

	2、cr 完成后，代码发送给值周同学

3、上线
	
	目前每周二、四上线两次，统一下午三点上线，下午二点需要代码 ready，超过时间等下次上线窗口



## 完整 demo 示例

```
<!DOCTYPE html>
<html mip>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
        <title>文章大标题</title>
        <link rel="stylesheet" type="text/css" href="https://mipcache.bdstatic.com/static/mipmain-v0.0.1.css">
    </head>
    <body>
    <mip-img src="http://a.hiphotos.baidu.com/image/h%3D200/sign=8aa71c7ce3c4b7452b94b016fffd1e78/3c6d55fbb2fb4316edd0e02f28a4462308f7d39c.jpg" popup alt="banner a" width="414" height="207" layout="responsive">
        <p>banner a</p>
    </mip-img>
    <mip-carousel autoplay defer="2000" width="414" height="207">
        <div>
            <mip-img src="http://a.hiphotos.baidu.com/image/h%3D200/sign=8aa71c7ce3c4b7452b94b016fffd1e78/3c6d55fbb2fb4316edd0e02f28a4462308f7d39c.jpg" popup alt="banner a" width="414" height="207" layout="responsive">
            </mip-img>
            <p>banner a</p>
        </div>
        <div>
            <mip-img src="http://img1.imgtn.bdimg.com/it/u=2363027421,438461014&fm=206&gp=0.jpg" popup alt="banner b" width="414" height="207" layout="responsive">
            </mip-img>
            <p>banner b</p>
        </div>
    </mip-carousel>
    <mip-demoforall></mip-demoforall>


    </body>
    <script src="https://mipcache.bdstatic.com/static/mipmain-v0.0.1.js"></script>
    <script src="https://mipcache.bdstatic.com/static/v0.1/mip-link.js"></script>
    <script src="https://mipcache.bdstatic.com/static/v0.1/mip-stats-bidu.js"></script>
    <script>
        
        /**
        * demo for 所有 mip 组件开发者
        * @exports modulename
        * @author lilangbo@baidu.com
        * @version 1.0
        * @copyright 2016 Baidu.com, Inc. All Rights Reserved
        */

        define('mip-demoforall', ['require', 'customElement'], function(require) {

            var customElem = require('customElement');

            customElem.prototype.init = function() {

                this.build = render;

            };

            function render() {
                if (this.isRender) {
                    return; 
                }

                this.isRender = true;
                
                console.log('test');
            }

            return customElem;
        });
        require(['mip-demoforall'], function (demoforall) {
            //注册组件
            MIP.registerMipElement('mip-demoforall', demoforall);
        });

    </script>
</html>

```