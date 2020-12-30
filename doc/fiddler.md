### Web开发调试利器--Fiddler
##### 一、Fiddler是个啥？
<p style="text-indent:2em">
  Fiddler是一个http协议调试代理工具，它能够记录并检查所有你的电脑和互联网之间的http通讯，设置断点，查看所有的“进出”Fiddler的数据（指cookie,html,js,css等文件）。 Fiddler 要比其他的网络调试器要更加简单，因为它不仅仅暴露http通讯还提供了一个用户友好的格式。
</p>

##### 二、Fiddler可以干啥？
<p style="text-indent:2em">
  笼统的说fiddler就是用来抓包的，同类的工具有: httpwatch, firebug, wireshark。主要是开发或者测试人员用来<b>分析</b>或者<b>修改</b>请求（request）、响应（response）包。
</p>

##### 三、安装以及设置
<p style="text-indent:2em">
  官网下载地址：<a href="https://www.telerik.com/download/fiddler" target="_blank">https://www.telerik.com/download/fiddler</a>；下载完成后运行安装，一路next就可以了。安装完之后打开，大概长这个样子
</p>
  <img src="https://861621821.github.io/xilan-docs/static/img/fiddler/全界面.png" >
<p style="text-indent:2em">
  我们需要简单的设置一下：点击导航栏Tools --> Options...<br>
</p>
  <img src="https://861621821.github.io/xilan-docs/static/img/fiddler/设置1.png" >
  <img src="https://861621821.github.io/xilan-docs/static/img/fiddler/设置2.png" ><br>
<p style="text-indent:2em">
  后面这个是让Fiddler可以抓取手机的包，<b>前提是手机与电脑在同一局域网</b>，另外需要手机设置代理，打开手机wifi设置，长按连接的局域网wifi，选择修改网络-显示高级选项-代理-手动。在开发H5页面的时候非常有用。<br>
</p>
  <img src="https://861621821.github.io/xilan-docs/static/img/fiddler/手机代理.jpg" style="width: 400px" ><br>
<p style="text-indent:2em">
  点击保存，在手机上打开任意网址试一试吧。    
</p>

##### 四、开始使用Fiddler
###### Inspectors
<p style="text-indent:2em">
  Inspectors栏功能跟浏览器的控制台的Network一样，点击任意请求，右边就会显示对应请求以及响应包的信息。
</p>
  <img src="https://861621821.github.io/xilan-docs/static/img/fiddler/inspectors.png" ><br>

###### AutoResponder
<p style="text-indent:2em">
  切换到AutoResponder栏，<b>记得勾选Enable rules、Unmatched requests passthrough</b>，否则不会生效。通过改包我们可以拿到任何我们想要的数据，即便是接口还没有开发完成。看到这里可能有小伙伴会想到mock，但是mock需要引入到项目里去，Fiddler无侵入，适用任何项目。下面是改包的详细步骤
</p>  
  
> 1.首先先抓取到请求，鼠标选择左边任意请求并拖至右边（注意这时候右边要处于AutoResponder栏位）  

  <img src="https://861621821.github.io/xilan-docs/static/img/fiddler/改包步骤1.png" ><br>  

> 2.鼠标右键点击规则，点击Edit Response，选择TextView，编辑你要改的数据，<strong>点击Save</strong>

  <img src="https://861621821.github.io/xilan-docs/static/img/fiddler/改包步骤3.png" >
  <img src="https://861621821.github.io/xilan-docs/static/img/fiddler/改包步骤2.png" ><br>  

_提示：Rule Editor里METHOD:POST EXACT:表示精确匹配，把它删掉可以模糊匹配。_

  <img src="https://861621821.github.io/xilan-docs/static/img/fiddler/精确匹配.png" ><br>
<p style="text-indent:2em">
  到这里，一个简单的改包就完成了，重新刷新页面看下效果，正常情况你的页面对应的地方就变成你改的数据了。左边匹配成功的请求会被标记为淡紫色背景，如果没有检查下Enable rules、Unmatched requests passthrough是否勾选了，或者查看下面的特殊情况。
</p>
<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/改包成功.png" ><br>
<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/成功后的数据.png" ><br>
<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/控制台结果.png" ><br>   
  
> 3.并不是只能处理接口数据，静态资源也是可以修改的。比如css，js，修改线上bug后，不用部署直接替换文件就可以看到效果了。

<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/改包静态文件.png" >  

<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/css改包.png" >

> 4.特殊情况
+ <p>编辑的时候发现是encode或者乱码状态，导致不能编辑</p>
<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/乱码.png" ><br>
<p>解决方法是在拖到右边前，先点击该请求，可以看到显示"Response body is encoded.Click to decode."，点击解码，然后再完成后面的操作。</p>
<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/解码.png" ><br>
+ <p>在正式请求前有一次预请求options，导致不能匹配到我们设置的规则</p>
<p>解决方法是把那条预请求也拖到右边</p>
+ <p>所有操作都正确，就是抓不到任何包：检查是否设置了其他代理，比如打开了谷歌上网助手</p>

###### Composer
<p style="text-indent:2em">
  Composer功能有点类似于postman，多用于测试接口。界面很简单，一目了然。编辑好之后点击Execute执行（这个也可以从左边拖过来）。
</p>  
<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/composer.png" ><br>  

###### FiddlerScript  
<p style="text-indent:2em">
  从字面上我们就可以看出FiddlerScript就是写Fiddler脚本的，Fiddler Script是基于JScript.NET写的。这才是Fiddler强大的地方。比如我们需要在开发阶段中由于数据原因，只有个别数据可以用来联调，我们就可以在FiddlerScript指定每次固定请求这个数据（当然你也可以在代码中写死，但是如果开发完忘记删除写死的参数，就引起了一个BUG），是不是逼格很高，也很保险。
</p>  

>  修改脚本前建议备份一下，免的玩坏了<br>  

<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/script1.png" ><br> 
我这个例子是不管点击任何文章都显示id为10030的文章内容，只是为了达到这个效果，脚本可能写的不优雅，勿喷。  
再举一个栗子，开发过程中有多个后台大佬给你写接口，在他们合并代码前，如果想同时调用A和B大佬的本地接口，除非写死请求的地址，否则应该是很困难的。利用Fiddler可以这样，不用改任何代码，方便又快捷  
<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/FiddlerScript.png" ><br>  

###### 设置断点
<p style="text-indent:2em">
  Fiddler提供了http请求断点机制，我们可以在请求发出前以及服务器返回数据后设置断点。可以通过点击按钮或者命令的方式设置断点，利用断点可以修改请求包或者响应包。
</p>    

<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/断点0.png" >  

>  命令方式  
> + 设置请求发出前断点：bpu xxx.com/api
> + 清除请求发出前断点：bpu 
> + 设置数据返回后断点：bpafter xxx.com/api
> + 清除数据返回后断点命令：bpafter 

手动输入只对命令后的地址设置断点，命令方式会对所有的请求设置断点。

+ 请求发出前断点  

图标状态为<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/前.png" class="nowh">  

<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/断点3.png" >  

执行完上述操作后可以看到，请求的参数已经被我们篡改了，response返回篡改后id的数据。

+ 数据返回后断点  

图标状态为<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/后.png" class="nowh">  

<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/断点4.png" >  

执行完上述操作后可以看到，response被我们篡改了，这样就可以模拟任何我们想要的数据。

##### 五、实用技巧
+ Ctrl + x 清空屏幕
+ Filter - Show only if URL contains 只显示符合的请求
+ 模拟弱网  

<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/弱网.png" ><br>
+ 选中请求右键保存，一般测试人员用来保留现场  

<img src="https://861621821.github.io/xilan-docs/static/img/fiddler/保存.png" ><br> 
##### 六、写在最后
<p style="text-indent:2em">
  Fiddler是一个很强大的工具，据说出过新闻说有人利用抓包软件低价充话费而获刑，我猜用的可能就是Fiddler 😂 😂 😂。我也只算入门，这篇文章仅仅是教新手上手使用，如果有精通Fiddler的大佬千万不要拿来做不法之事。
</p> 