# 微信支付二维码
为了统一微信支付场景的样式，封装了此组件，只对样式的封装不涉及逻辑。
#### 使用
``` html
<ly-wechat-qr :loading="loading" :total="total" :error="error" @refresh="handleRefresh"/>
```  

#### Attributes
| 属性  | 类型    | 说明 |  默认值 |
|---|---|---|---|
|loading|Boolean|是否处于loading中|false|
|total|String|金额|-|
|error|Boolean|异常|false|

#### Events
| 事件名  | 说明    | 参数 |
|---|---|---|
|refresh|刷新按钮被点击时的回调|-|  
