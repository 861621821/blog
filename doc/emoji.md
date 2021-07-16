# 微信emoji表情
最近公司项目中需要开发一个支持微信表情的功能模块，就动手写了个这样的组件。微信中的表情功能是微信团队自己开发的一套'emoji'，其实就是`[微笑]`这样的字符串，而这个字符串在微信上被替换成了笑脸图。  

[代码地址](https://github.com/861621821/repository/tree/master/emoji)  
#### 示例
![emoji](https://xilanjs.com/images/component/emoji.gif)
#### 使用
```js
import lyEmoji from '@/components/emoji/index.vue'
import emoji from '@/components/emoji/index.vue'
export default {
...
  components: { lyEmoji },
  directives: {
    emoji
  },
...
  methods: {
    handleEmojiChange (emoji) {
      this.msg = this.msg + emoji
    }
  }
}
```
```html
<ly-emoji @change="handleEmojiChange">
  <!-- 默认插槽：打开弹窗的icon -->
  <img src="@/assets/images/xiaolian.png">
</ly-emoji>
<!-- 因为添加表情实际上是添加[微笑]字符串，所以用自定义指令将字符串替换成表情，如果你不需要显示表情则不需要使用指令 -->
<div v-emoji="msg"></div>
```  
!> 提示：如果想把输入框内的表情字符串也替换成表情，可以自己模拟一个输入框