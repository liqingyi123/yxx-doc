# 更新日志 - Release Notes

## 1.2111.1
**2021年11月25日** *[预上线测试阶段]*

* 新增 免单中心增加今日免单次数气泡
* 优化 日历组件s-calendar回调属性更改为`v-model`，无需再触发手动触发回调来通知父组件需要更新折叠展开状态
* 修复 免单兑换完成后，剩余兑换次数没有自动减一
* 优化 当接口请求发生错误时增加请求参数日志输出
* 修复 当接口验证出`6007`、`6006`、`6001`或`12345`错误码时，点击确定没有自动跳转登录页重新登录