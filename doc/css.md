### CSS技巧  
##### * 左图右文  
``` html
<div>
  <img src="https://861621821.github.io/repository/img/css/人.png">
  <p>我们所要介绍的是祥子，不是骆驼，因为“骆驼”只是个外号；那么，我们就先说祥子，随手儿把骆驼与祥子那点关系说过去，也就算了。北平的洋车夫有许多派：年轻力壮，腿脚灵利的，讲究赁漂亮的车，拉“整天儿”，爱什么时候出车与收车都有自由；拉出车来，在固定的“车口”①或宅门一放，专等坐快车的主儿；弄好了，也许一下子弄个一块两块的；碰巧了，也许白耗一天，连“车份儿”也没着落，但也不在乎。这一派哥儿们的希望大概有两个：或是拉包车；或是自己买上辆车，有了自己的车，再去拉包月或散座就没大关系了，反正车是自己的。</p>
</div>
```
``` css
img{
  float: left
}
p{
  overflow: hidden
}
```
<b>效果：</b>
<div>
  <img src="https://861621821.github.io/repository/img/css/人.jpg" style="width: 100px; margin-right: 20px; float: left">
  <p style="width: 400px;overflow: hidden">我们所要介绍的是祥子，不是骆驼，因为“骆驼”只是个外号；那么，我们就先说祥子，随手儿把骆驼与祥子那点关系说过去，也就算了。北平的洋车夫有许多派：年轻力壮，腿脚灵利的，讲究赁漂亮的车，拉“整天儿”，爱什么时候出车与收车都有自由；拉出车来，在固定的“车口”①或宅门一放，专等坐快车的主儿；弄好了，也许一下子弄个一块两块的；碰巧了，也许白耗一天，连“车份儿”也没着落，但也不在乎。这一派哥儿们的希望大概有两个：或是拉包车；或是自己买上辆车，有了自己的车，再去拉包月或散座就没大关系了，反正车是自己的。</p>
</div>  

##### * 文字环绕图片  
``` html
<div>
  <img src="https://861621821.github.io/repository/img/css/人.png">
  <p>我们所要介绍的是祥子，不是骆驼，因为“骆驼”只是个外号；那么，我们就先说祥子，随手儿把骆驼与祥子那点关系说过去，也就算了。北平的洋车夫有许多派：年轻力壮，腿脚灵利的，讲究赁漂亮的车，拉“整天儿”，爱什么时候出车与收车都有自由；拉出车来，在固定的“车口”①或宅门一放，专等坐快车的主儿；弄好了，也许一下子弄个一块两块的；碰巧了，也许白耗一天，连“车份儿”也没着落，但也不在乎。</p>
</div>
```
``` css
img{
  float: left
}
```
<b>效果：</b>
<div>
  <img src="https://861621821.github.io/repository/img/css/人.jpg" style="width: 100px; margin: 0 20px 20px 0; float: left">
  <p style="width: 400px;">我们所要介绍的是祥子，不是骆驼，因为“骆驼”只是个外号；那么，我们就先说祥子，随手儿把骆驼与祥子那点关系说过去，也就算了。北平的洋车夫有许多派：年轻力壮，腿脚灵利的，讲究赁漂亮的车，拉“整天儿”，爱什么时候出车与收车都有自由；拉出车来，在固定的“车口”①或宅门一放，专等坐快车的主儿；弄好了，也许一下子弄个一块两块的；碰巧了，也许白耗一天，连“车份儿”也没着落，但也不在乎。</p>
</div>  

##### * 毛玻璃效果
``` html
<div class="parent">
  <div class="child">毛玻璃效果</div>
</div>
```
``` css
.child{
  background: rgba(255, 255, 255, 0.3);
  backdrop-filter: blur(2px);
}
```
<b>效果：</b>
<div class="parent" style="height: 100px; padding: 10px; background: url('https://861621821.github.io/repository/img/css/人.jpg') center repeat">
  <div class="child" style="background: rgba(255, 255, 255, 0.3);backdrop-filter: blur(2px); line-height: 80px ;text-align: center">毛玻璃效果</div>
</div>  

> 自身半透明是关键  

##### * object-fit实现img标签类似background的cover样式
``` css
img{
  width: 100px;
  height: 100px
  object-fit:cover;
}
```
<b>效果(使用前与使用后)：</b>  
<img src="https://861621821.github.io/repository/img/css/人01.jpg" style="width: 100px; height: 100px">
<img src="https://861621821.github.io/repository/img/css/人01.jpg" style="object-fit:cover; width: 100px; height: 100px">  

##### * 美化滚动条
``` css
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}

::-webkit-scrollbar-track {
  width: 6px;
  background: rgba(#101F1C, 0.1);
  -webkit-border-radius: 2em;
  -moz-border-radius: 2em;
  border-radius: 2em;
}

::-webkit-scrollbar-thumb {
  background-color: rgba(#101F1C, 0.5);
  background-clip: padding-box;
  min-height: 28px;
  -webkit-border-radius: 2em;
  -moz-border-radius: 2em;
  border-radius: 2em;
}

::-webkit-scrollbar-thumb:hover {
  background-color: rgba(#101F1C, 1);
}
```
##### * 块级格式化上下文BFC  
1.同一个BFC内相邻的元素在垂直方向外边距重叠  
2.计算BFC高度时，浮动的元素也参与计算
3.BFC是一个独立的容器，里外元素互补影响
生产BFC的方法：
1.overflow: hidden/auto
2.display: inline-block
3.position: absolute/fiexd
