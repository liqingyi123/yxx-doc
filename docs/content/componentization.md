# 组件化
## UI组件
### 自编写组件
?> 已使用easycom规则自动导入，使用时无需再次导入相应组件
#### 日历 s-calendar
> 代码示例

```html
<s-calendar :dotDays="hasStepDays" start="2021-8-1" @change="dayChange" v-model="putAway" />
```
```js
data(){
    return{
        hasStepDays: [1,2,6,10],//有足迹的那几天1号2号6号10号
        putAway: true
    }
},
methods: {
    dayChange: function(event){
        if(event.year!=this.nowDate[0] || event.month!=this.nowDate[1]){
            this.isToday = event.year==nowDate[0]&&event.month==nowDate[1]
            this.nowDate = [event.year,event.month,event.day]
            this.getFootSteps();
        }else{
            event.value = String(event.year)+String(event.month>9?event.month:'0'+event.month)+String(event.day>9?event.day:'0'+event.day)
            const hasObj = findIndexFromArray(saveStepList,event.value,'date')
            this.setData({
                stepList: hasObj ? [hasObj.obj] : []
            })
        }
    },
}
```
> 参数 - props

| 字段 | 类型 | 必填 | 默认值 | 用途 |
|:---:|:---:|:---:|:---:|:---|
| startWithMonday | Boolean | 否 | false | 周起始日为周一，默认以周日 |
| lineCount | String/Number | 否 | 5 | 单月限时行数 |
| fontSize | String/Number | 否 | 30 | 字体大小 |
| activeStyle | Object | 否 | {} | 当前天的样式 |
| isChoice | Boolean | 否 | true | 日期是否可选 |
| currentDay | String/Number | 否 | true | 当前选中的日期，不传则默认当天 |
| dotDays | Array | 否 | [] | 显示底部提示点的几天，传入数字数组即可 |
| dotColor | String | 否 | #E63131 | 提示点颜色 |
| bgColor | String | 否 | #FFFFFF | 整体背景色 |
| start | String | 否 | 2021-2-1 | 开始时间 |
| end | String | 否 | 当天的时间 | 结束时间 |
| acrossSelect | Boolean | 否 | true | 否可以跨月选择 |
| v-model | Boolean | 否 | false | 是否折叠日历 |
> 事件 - events

| 事件名 | 参数 | 返回值 | 用途 |
|:---:|:---:|:---:|:---|
| change | 无 | {value,year,month,day} | 日期改变后触发 |
#### 空状态 s-empty
> 代码示例

```html
<s-empty :isshow="list.length==0" />
```
> 参数 - props

| 字段 | 类型 | 必填 | 默认值 | 用途 |
|:---:|:---:|:---:|:---:|:---|
| isshow | Boolean | 否 | true | 是否显示 |
| mode | String | 否 | default | 空状态类型，可选值为`default`、`collect`、`network`、`order`、`search`、`shop` |
| content | String | 否 |  | 提示文字 |
#### 商品列表 s-goodslist
> 代码示例

```html
<s-goodslist :list="nowFreeList" theme="freeZone" :free-list-type="tabIndex!=0?'不是正在免单':''" @itemclick="itemClick" :free-error-txt="freeErrorTxt"></s-goodslist>
```
```js
data(){
    return{
        nowFreeList: [],
        freeErrorTxt: {}
    }
},
methods: {
    itemClick: function(event){
        nowCheckGoods = event.item;
        if(this.tabIndex==0){
            checkUserBlack(()=>{
                if(event.item.status == 1){
                    if(this.lastFreeCount){
                        this.setData({
                            ['maskInfo.type']: 'sureBuy',
                            ['maskInfo.types.sureBuy.info']: nowCheckGoods,
                            showMask: true
                        });
                    }else{
                        this.setData({
                            ['maskInfo.type']: 'countEmpty',
                            showMask: true
                        });
                    }
                }
            })
        }else if(this.tabIndex==1){
            if(event.item.status == 1 || event.item.status == 2){//审核中或待收货
                this.setData({
                    ['maskInfo.type']: 'unReceived',
                    showMask: true
                });
            }else if(event.item.status == 3){//去兑换
                if(event.item.commentImage){
                    this.exChangeOrder();
                }else{
                    this.setData({
                        ['maskInfo.type']: 'upload',
                        showMask: true
                    });
                }
            }
        }
    }
}
```
> 参数 - props

| 字段 | 类型 | 必填 | 默认值 | 用途 |
|:---:|:---:|:---:|:---:|:---|
| list | Array | 是 | [] | 表数据，可以传一维或二维数组，推荐传二维数组以提高性能 |
| theme | String | 否 | default | 列表主题，可选值为`default/single`默认、`double`双列模式、`border-right`右下角边框样式、`seckill`限时秒杀、`freeZone`免单 |
| showCollectBtn | Boolean | 否 | true | 是否显示收藏按钮-暂只有默认列表有效 |
| freeListType | String | 否 | 免单购 | 免单列表类型 正在免单或已结束待兑换 |
| itemClickEvent | Boolean | 否 | true | 是否开启点击事件回调，默认开启 |
| freeErrorTxt | Object | 否 | {'1': '该补贴还未开启兑换，请耐心等待哦','2': '该补贴商品未收货，请先收货','4': '该补贴商品指定时间未兑换，如有疑问请联系客服'} | 当为不可兑换状态时的点击提示语，1审核中，2待收货， 3去兑换， 4已过期 |
> 事件 - events

| 事件名 | 参数 | 返回值 | 用途 |
|:---:|:---:|:---:|:---|
| itemclick | 无 | {index,item,father} | 列表项点击后触发 |
| collectChange | 无 | {index,item,father} | 列表项点击快速收藏后触发 |
#### 导航栏 s-navbar
> 代码示例

```html
<s-navbar :showBack="false" bgColor="#E63131" position="relative" height="auto">
    <view class="header">
        <view class="title f-r ai-end">
            <view>热销榜单</view>
            <view>热卖，大家都在推</view>
        </view>
        <view class="filter f-r-itemCenter">
            <view v-for="(item,index) in filters" :key="index" class="f-r-itemCenter" :class="[filterIndex==index?'check':'']" @click="filterChange(index)">
                <image src="/static/icon/system/hotSale/hot_tab.png" />{{item.name}}热销榜
            </view>
        </view>
    </view>
</s-navbar>
```
> 参数 - props

| 字段 | 类型 | 必填 | 默认值 | 用途 |
|:---:|:---:|:---:|:---:|:---|
| height | Number/String | 否 | 45 | 高度 |
| title | String | 否 |  | 标题文字，当插槽不为空时则不生效 |
| showBack | Boolean | 否 | true | 是否显示返回按钮 |
| backStyle | String | 否 |  | 返回按钮自定义图标 |
| backSize | Number/String | 否 | 36 | 返回按钮大小 |
| defaultBack | String | 否 | /pages/main/home/home | 默认返回失败后的路径 |
| bgColor | String | 否 | #FFFFFF | 背景色 |
| position | String | 否 | fixed | 定位，同css的position属性 |
| eventBack | Boolean | 否 | false | 开启事件托管 |
> 事件 - events

| 事件名 | 参数 | 返回值 | 用途 |
|:---:|:---:|:---:|:---|
| routeback | 无 | 无 | 返回按钮事件托管 |
#### 排序筛选器 s-sorter
> 代码示例

```html
<s-sorter offsetTop="98" @change="sortChange" hasSort />
```
```js
methods: {
    sortChange: function(event){
        if(event.type === 'switch'){
            this.couponSwitch = event.value
        }else{
            this.sort = event.item.sort!=undefined?event.item.id[event.item.sort]:event.item.id
        }
        this.goodsList = [];
        this.pageNum = 1;
        this.listEnd = false;
        this.searchSubmit();
    }
}
```
> 参数 - props

| 字段 | 类型 | 必填 | 默认值 | 用途 |
|:---:|:---:|:---:|:---:|:---|
| offsetTop | String/Number | 否 | 0 | 吸顶的距离，单位rpx |
| hasCouponSwitch | Boolean | 否 | true | 是否含有券过滤 |
| hasSort | Boolean | 否 | false | 是否有排序规则 |
| tabs | Array | 否 | [{id: 0,name: '综合'},{id: [3,4],name: '价格',sort: ''},{id: [5,6],name: '销量',sort: ''}] | 排序依据 |
> 事件 - events

| 事件名 | 参数 | 返回值 | 用途 |
|:---:|:---:|:---:|:---|
| change | 无 | {type:'tab',index,item}/{type:'switch',value} | 排序过滤规则改变时触发 |
### 第三方组件
?> 已使用easycom规则自动导入，使用时无需再次导入相应组件

使用uView UI 1.0，使用组件时以u-开头，具体请参考[uView官方文档](https://v1.uviewui.com/components/intro.html)

## 工具封装
### 全局通用工具库 util.js
| 函数名 | 用途 |
|:---:|:---|
| ossEncryption | 阿里云oss上传前准备 |
| request | 封装请求 |
| requestSynchro | 同步请求 |
| goBackPage | 通用的返回按钮执行函数 |
| goToPage | 通用的跳转页面函数 |
| formatTime | 日期格式转换 |
| formatNumber | 数字格式化 |
| enCodeAESData | aes加密 |
| deCodeAESData | aes解密 |
| findArrayFromArray | 查找并返回数组中是否存在某一个值的新数组 |
| findIndexFromArray | 查找并返回数组中某一个值的下标 |
| getAlbumInfo | 相册授权 |
| jsonToQuery | json转query |
| queryToJson | query转json |
| downFiles | 批量下载文件 |
| saveVedioFile | 保存MP4视频文件到文件系统 |
| chooseLocalImgTobase64 | 选择本地图片并转base64数据 |
| Is | 判断是否是目标类型 |
| bubbleSort | 冒泡排序优化版 稳定，速度慢，不适用大量数据 |
| mergeSort | 归并排序 稳定，速度快，适用大量数据，占额外内存 |
| copyText | 复制文本到剪贴板 |
| savePicToPhotosAlbum | 保存图片到相册 |
| Debounce | 防抖函数 只触发最后一次的事件 |
| Throttle | 节流函数 每隔固定时间触发一次 |
| modifyItemToObject | 向原对象/数组中添加/修改某一个键值并返回 |
| chooseLocalImgTo | 选择本地图片并转成相应格式 |
| deepCopy | 深拷贝 |
| starTxt | 加密过滤器 |
| formatLastTime | 计算距离现在多长时间以前 |
| goPddPlug | 跳转拼多多插件页 |
### 本项目专用工具库 public.js
| 函数名 | 用途 |
|:---:|:---|
| createSharePath | 创建分享路径 |
| quickNav | 快捷导航 |
| beforePageLoad | 页面初始化时的数据填充 |
| activeMaskList | 获取活动弹窗和浮窗列表并自动装填弹窗和浮窗 |
| goGoodsDetail | 跳转商品详情页 |
### 本项目专用数据请求 getData.js
| 函数名 | 用途 |
|:---:|:---|
| getPopupList | 获取弹窗列表并自动弹出 |
| lookPopupDetail | 打开弹窗 |
| reportPageVisit | 页面访问统计 |
| reportSearchKeyClick | 关键字搜索统计 |
| getPddGoodsBuyLink | 生成购买链接 |
### 正则字典 regular.js
| 正则名称 | 用途 |
|:---:|:---|
| name | 中文名字 |
| phone | 手机号和电话号通用 |
| postalCode | 邮政编码 |
| email | 电子邮箱 |
| id | 身份证 |
| id18 | 18位身份证 |
| id15 | 15位身份证 |
| ipAddress | ip地址 |
| chinese | 中文字符 |
| url | url地址 |
| rankCard | 银行卡 |
| mobilePhone | 手机号 |
| address | 住址(需要满足2个以上的地址属性，例：三元村嘎达屯) |
| hostName | 主机名 |
| doubleByteChar | 双字节字符，即汉字 |
| floatChar | 浮点数 |
| intChar | 整数 |
| QQNumber | QQ号 |
| number | 数字 |
| englishAndNumber | 英文字母或数字 |
| telephone | 电话号码 |
| strongPwd | 强密码(必须包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间) |
| noneLine | 可以用来删除空白行 |
| htNone | 可以用来删除行首行尾的空白字符(包括空格、制表符、换页符等等) |
| xml | xml文件(###.xml或###.XML) |
| html | html标记 |
| charAndChinese | 字母和汉字 |
| filePath | 文件路径 |