* split一个需要注意的小地方    
> 空字符串""在调用split分隔符为非""的情况下返回的是一个[""], 这是一个有值的数组，和预期的结果[]是不一样的，需要做一下容错处理
```javascript
  var result = stem ? stem.split(',') : []
```

* axios在处理请求体对象属性值为空或者undefined的区别    
> 比如{a: undefined, b: ""}, 在最终的请求体中，属性为undefined的属性会被忽略，而属性值为空字符串的，将会被保留

* JSON.parse()是个不太安全的方法    
> 如果parse的参数不是标准的JSON字符串，那么parse方法会报错，并且导致整个应用中断, 在使用这个方法前，尽量判断一下参数是否满足要求。<br/>[当JSON.parse遇上非键值对](https://juejin.im/post/6844903661651542029)