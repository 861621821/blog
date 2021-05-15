### 简单实现一个防抖与节流函数
##### debounce
``` js
function debounce(cb, time){
  var timer = null
  return function () {
    if(timer){
      clearTimeout(timer)
    }
    timer = setTimeout(cb, time)
  }
}
```
##### throttle
``` js
function throttle(cb, t){
  var time = null
  return function(){
    var currentTime = Date.now()
    if(!time || currentTime - time >= t){
      cb()
      time = currentTime
    }
  }
}
```