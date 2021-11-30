# 文档
## 使用说明
1. **页面跳转不建议使用官方的4种页面跳转**，请使用util.js中的`goToPage`代替`switchTab`、`redirectTo`、`navigateTo`；使用`goBackPage`代替`navigateBack`，具体用法参照[对应文件](/content/componentization.md#全局通用工具库-utiljs)说明

2. **缓存问题**

> 网页分活动网页和一般网页，框架会自动在网页加上?v=时间戳&token=用户token

3. **关于setdata优化**

> 当数据为数组时，推荐采用二维数组，嵌套遍历，此项目用的最多的是商品列表，本框架作者封装了一个[商品列表组件](/content/componentization.md#商品列表-s-goodslist)，可以自动识别开发者传入的是一维数组还是二维数组

4. **utils文件夹说明**

- official 官方/依赖工具，具体内部工具不作说明
- self 私有工具，由开发者自己维护
    - api 接口映射表
    - console 控制台日志美化
    - ctxTool 绘图板画笔相关工具（暂未使用）
    - filter 过滤器集群
    - getData 本项目专用数据请求
    - public 本项目专用工具库，仅针对当前项目有效的常规工具
    - regular 正则字典
    - sign 签名生成
    - tool 私有工具，一般用在不会大规模使用的地方如app初始化和接口错误码拦截
    - util 全局通用工具库，不依赖于项目的工具集合
5. **pages文件夹说明**

- main 主包 存放基本页面（tabbar页、登陆注册页、图片资源等）
- subpacks 分包
    - active 活动类页面（通用H5、礼金、千万补贴等）
    - PDDActivePlug 拼多多活动落地页插件包
    - pddPlug 拼多多插件包
    - qySDK 七鱼客服插件包
    - system 系统类页面（搜索、会场、系统设置、关于、问答等）
    - user 用户类页面（收藏、资料、足迹等）
- router.js (页面路由映射表)
- tabbar 自定义tabbar组件
6. **页面路由映射**

pages目录下的`router.js`文件为路由文件,本框架中所有页面跳转都会用到，util.js中的`goToPage`和`navigateBack`的路径解析，包括后台配置的页面路径://home?id=123都需要此文件支持，建议每新建一个页面，相应的增加一条记录

7. **邀请分享动作统一配置**

> 请使用public.js中的`createShareCard`自动拼装分享路径函数进行分享卡片内容定制

8. **优化限频接口**

如`uni.login`、`uni.checkSession`、`uni.getSetting`、`uni.getUserInfo`、`uni.getUserProfile`
- 把上一次调用接口的返回结果缓存下来以供后续逻辑复用，而不是重新调用接口
- 避免在定时循环的逻辑内重复调用“限频接口”
- 尽量避免在页面初始化事件`onLoad`、`onShow`、`onReady`中调用限频接口，应该在app.vue初始化事件中调用

9. **本框架封装了同步请求**

当有同步请求业务时，可以使用以async+await标识进行包装或者使用回调的方式
```js
async reload(){
    await request1();
    await request2();
    await request3();
}
request1(()=>{
    request2(()=>{
        request3(()=>{})
    })
})
```
10. **页面参数处理函数封装**

页面的参数处理建议使用public.js里面的`beforePageLoad`函数，此函数会自动将user和shareId装载进页面data中，前提是在data中要先声明，具体用法参照[对应文件](/content/componentization.md#本项目专用工具库-publicjs)说明。

11. **日志美化**

本框架作者封装并集成了日志美化功能，内置init/pageLoad/quickNav/request/lo级别的日志
- init：(绿色标头)程序入口文件初始化时执行，没有实际含义，只是本框架搭建者的声明
- pageLoad：(绿色标头)当页面onLoad调用公用页面初始参数预处理函数时，会自动使用此日志，方便开发者查看该页面经过处理前后的参数有哪些
- quickNav：(紫色标头)当使用banner、icon等进行quickNav快捷导航时，会自动使用此日志，方便开发者查看当前跳转参数有哪些
- request：(蓝色/橙色标头)当请求产生结果时，会自动使用此日志，当请求成功时会是蓝色标头，失败则会是橙色标头，建议开发者在使用公用request函数时传入title字段以区别每个请求的用途
- lo：(紫色标头)【开发者后期日志打印请使用此级别】建议开发者摒弃旧的console.log打印日志，改为console.lo
例如：
```js
console.lo('我是日志内容')
console.lo('我是标题','我是日志内容')
console.lo('我是红色的标题','我是日志内容,我后面是这条日志的颜色','#FF5042')
console.lo('我是标题','我是日志内容,我后面也是日志内容','FF5042')
console.lo({a:1},'我是日志内容,我前面也是日志内容')
```

## 效率化方案

?> 为了提高开发效率，请复制以下代码片段到开发工具中。
* 微信开发者工具 文件-首选项-用户代码片段
* HbuilderX 工具-代码块设置
* vscode 左下角设置-用户代码片段

javascript.json文件中：
```js
{
    "快速生成方法体": {
        "prefix": "func",
        "body": [
            "function(){",
            "    $1",
            "};"
        ],
        "description": "快速生成方法体"
    },
    "快速生成函数变量": {
        "prefix": "func:",
        "body": [
            "$1: function($2){",
            "    $3",
            "},"
        ],
        "description": "快速生成函数变量"
    },
    "快速生成箭头函数": {
        "prefix": "jt=",
        "body": [
            "$1=>{",
            "    $2",
            "};"
        ],
        "description": "快速生成箭头函数"
    },
    "快速生成按需导入": {
        "prefix": "req",
        "body": [
            "const {",
            "    $2",
            "} = require('$1');"
        ],
        "description": "快速生成按需导入"
    },
    "快速日志,级别lo": {
        "prefix": "log",
        "body": [
            "console.lo('$1',$2)"
        ],
        "description": "快速日志,级别lo"
    },
    "快速日志,级别pageLoad": {
        "prefix": "logpageLoad",
        "body": [
            "console.pageLoad('$1',$2,$3)"
        ],
        "description": "快速日志,级别pageLoad"
    },
    "快速日志,级别quickNav": {
        "prefix": "logquickNav",
        "body": [
            "console.quickNav($1)"
        ],
        "description": "快速日志,级别quickNav"
    },
    "快速日志,级别request": {
        "prefix": "logrequest",
        "body": [
            "console.request('$1',$2,$3)"
        ],
        "description": "快速日志,级别request"
    },
    "request请求体": {
        "prefix": "request",
        "body": [
            "request({",
            "    title: $1,",
            "    url: $2,",
            "    data: $3,",
            "    success: res=>{",
            "        if(res.code == 0){",
            "            $4",
            "        }",
            "    }",
            "});"
        ],
        "description": "request请求体"
    },
    "forEach遍历": {
        "prefix": "fore",
        "body": [
            "$1.forEach((el$2)=>{",
            "    $3",
            "});"
        ],
        "description": "forEach遍历"
    },
    "setData数据装填": {
        "prefix": "setdata",
        "body": [
            "this.setData({",
            "    $1",
            "},()=>{",
            "    $2",
            "});"
        ],
        "description": "setData数据装填"
    },
    "dataset取值": {
        "prefix": "dataset",
        "body": [
            "const $1 = event.currentTarget.dataset.$2"
        ],
        "description": "dataset取值"
    },
    "通用跳转页面": {
        "prefix": "gopage",
        "body": [
            "goToPage({",
            "    url: $1,",
            "    traceless: $2",
            "});"
        ],
        "description": "通用跳转页面"
    },
    "返回上一级页面": {
        "prefix": "backpage",
        "body": [
            "goBackPage($1);"
        ],
        "description": "返回上一级页面"
    },
    "防抖函数": {
        "prefix": "fd",
        "body": [
            "$1: Debounce(($2)=>{",
            "    $4",
            "}$3);"
        ],
        "description": "防抖函数"
    },
    "节流函数": {
        "prefix": "jl",
        "body": [
            "$1: Throttle(($2)=>{",
            "    $4",
            "}$3);"
        ],
        "description": "节流函数"
    },
    "页面数据处理": {
        "prefix": "beforepageload",
        "body": [
            "beforePageLoad({",
            "    pageTitle: '$1',",
            "    pageNode: this,",
            "    premiseKey: $2,",
            "    premiseType: '&&',",
            "    options,",
            "    unPremiseFun: ()=>{",
            "        uni.showModal({",
            "            content: '参数异常',",
            "            showCancel: false,",
            "            success: ()=>{",
            "                goBackPage(globalData.router['home'].path)",
            "            }",
            "        });",
            "        $3",
            "    },",
            "    adoptFun: pageSet=>{",
            "        $4",
            "    }",
            "});"
        ],
        "description": "页面数据预处理"
    }
}
```
html.json文件中：
```js
{
    "插入图片": {
        "prefix": "image",
        "body": [
            "<image src=\"$1\" mode=\"aspectFit\"/>"
        ],
        "description": "插入图片"
    },
    "插入类view": {
        "prefix": "div.",
        "body": [
            "<view class=\"$1\">$2</view>"
        ],
        "description": "插入图片"
    },
    "插入唯一view": {
        "prefix": "div#",
        "body": [
            "<view id=\"$1\">$2</view>"
        ],
        "description": "插入图片"
    },
    "插入轮播组件": {
        "prefix": "swiper",
        "body": [
            "<swiper autoplay circular interval=\"{{3000}}\">",
            "    <swiper-item v-for=\"(item,index) in $1\" :key=\"index\">",
            "        <image src=\"{{item}}\" />",
            "        <view class=\"\"></view>",
            "        $2",
            "    </swiper-item>",
            "</swiper>"
        ],
        "description": "插入图片"
    },
    "空状态-自定义组件": {
        "prefix": "empty",
        "body": [
            "<s-empty isshow=\"{{$1}}\" />"
        ],
        "description": "空状态-自定义组件"
    },
    "遍历": {
        "prefix": "for",
        "body": [
            "v-for=\"(item,index) in $1\" :key=\"index\""
        ],
        "description": "遍历"
    },
    "遍历-自定义字段": {
        "prefix": "fork",
        "body": [
            "v-for=\"($2,$3) in $1\" :key=\"$3\""
        ],
        "description": "遍历-自定义字段"
    }
}
```