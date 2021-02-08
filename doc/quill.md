### vue-quill-editor增加截图粘贴图片功能  
直接上代码
``` js
this.$refs.quill.$el.addEventListener('paste',this.handlePaste)

...

handlePaste(e){
  const cbd = e.clipboardData||window.clipboardData
  var ua = window.navigator.userAgent;
  // 如果是 Safari 直接 return
  if ( !(e.clipboardData && e.clipboardData.items) ) {
    return
  }
  // Mac平台下Chrome49版本以下 复制Finder中的文件的Bug Hack掉
  if(cbd.items && cbd.items.length === 2 && cbd.items[0].kind === "string" && cbd.items[1].kind === "file" &&
    cbd.types && cbd.types.length === 2 && cbd.types[0] === "text/plain" && cbd.types[1] === "Files" &&
    ua.match(/Macintosh/i) && Number(ua.match(/Chrome\/(\d{2})/i)[1]) < 49){
    return
  }
  for(var i = 0; i < cbd.items.length; i++) {
    var item = cbd.items[i]
    if(item.kind == "file"){
      var blob = item.getAsFile();
      if (blob.size === 0 || !blob.type.match('image')) {
        return
      }
      var files = new File([blob], Date.now()) // 转为file 修改文件名
      this.uploadImg(files) // 项目中上传图片的方法
    }
  }
}
```