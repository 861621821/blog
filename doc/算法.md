+ ##### 确定只有一个数字出现一次，其他均出现两次，返回这个数字  
```js
const arr = [1,1,2,2,3,4,4]
function fn(arr){
    var temp = arr[0]
    for(let i = 1; i < arr.length; i++){
      temp ^= arr[i]
    }
    return temp
}
fn(arr)
```  
