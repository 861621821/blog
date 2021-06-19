## 通过npm简化本地文件的引用
我们都知道npm是node的包管理器，基本上都是用它来下载依赖包和运行项目，它还有一些不常用但很实用的功能，比如`npm link`、`npm install file:`通过npm简化本地文件的引用。例如项目中有一个公共js，文件路径为`/src/common/index.js`，如果要引用这个js需要`import xxx from "../../common/index"`。  
> 这样写有几个缺点：  
>+ 麻烦、难看（需要一级一级的找到文件）
>+ 难以维护（一旦修改了路径，所有的地方都需要改）  

#### npm link
`npm link`专门用于开发和调试本地npm模块，能做到在不发布模块的情况下，把本地的一个正在开发的模块的源码链接到项目的`node_modules`目录下，让项目可以直接使用本地的npm模块
###### 步骤
1.文件结构不变（`/src/common/index.js`），在`common`目录下创建`package.json`  
2.在`package.json`中编辑以下信息
``` js
{
  "name": "common", // name随意，引用的时候就是import xxx from "common"
  "version": "1.0.0",  // 版本号
  "description": "微前端公共模块",  // 描述信息
  "main": "index.js"  // 入口文件
}
```  
3.在`common`目录下执行`npm link`，把本地模块注册到全局  
4.在项目根目录下，执行`npm link common`，把`common`链接到项目中的`node_modules`下<br>
5.现在去项目中通过`import xxx from "common"`试一下  
#### npm install file:
直接把本地模块安装到项目的`node_modules`中，同时写入`package.json`依赖中，别人clone项目，直接执行install就能使用了。
###### 步骤
与`npm link`不同的是，不需要执行步骤3、4，而是在项目根目录下执行`npm install file:./src/common -S`，快去试一下吧。