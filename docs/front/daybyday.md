<!--
 * @Description: 
 * @Version: 1.0.0
 * @Autor: yin gang
 * @Date: 2020-09-28 16:37:42
 * @LastEditors: yin gang
 * @LastEditTime: 2020-11-07 14:54:10
-->
* split一个需要注意的小地方    
> 空字符串""在调用split分隔符为非""的情况下返回的是一个[""], 这是一个有值的数组，和预期的结果[]是不一样的，需要做一下容错处理
```javascript
  var result = stem ? stem.split(',') : []
```

* axios在处理请求体对象属性值为空或者undefined的区别    
> 比如{a: undefined, b: ""}, 在最终的请求体中，属性为undefined的属性会被忽略，而属性值为空字符串的，将会被保留

* JSON.parse()是个不太安全的方法    
> 如果parse的参数不是标准的JSON字符串，那么parse方法会报错，并且导致整个应用中断, 在使用这个方法前，尽量判断一下参数是否满足要求。<br/>[当JSON.parse遇上非键值对](https://juejin.im/post/6844903661651542029). 可以写一个全局方法，在方法里面对JSON.parse 作try catch处理

* 一些解析url对象的方法，在参数值=0或者1的情况下，比如is_collect=0，接触出来的对象属性值是字符串 {is_collect: '0'}, 如果需要做布尔值的真假判断就需要注意一下 "0" 和 0 的区别

* 代码里面的无意义字符很大程度上会阻碍后续的人梳理清楚业务，特别是用在分支判断的时候，比如 type == 1 type == 2 这种，在接手的人不了解业务情况下根本就不知道这两个分支所对应的业务场景，一个解决方法是用有意义的常量来代替无意义的数值，比如 type == PAY_SUCCESS type = PAY_FAIL type = PAY_ABORT ，一定程度上可以方便区分，如果场景和因素比较多，不方便用常量描述，则有必要用注释来进行说明。

* try catch的时候，如果在catch里面又throw了错误, 比如 throw new TypeError('is not a variable'), 还是会中断后续程序运行的。所以为了不中断，catch里面就不要再抛出错误了。

* 安全的判断一个变量是否已定义用 typeof

* 在用v-html时发现的问题，如果v-html的值里面包含有格式符号比如 \n \t \r ,不处理的情况，v-html渲染出来是没有格式的。如果需要有格式，则需要给v-html的元素加上 white-space: pre-wrap (保留空白符，正常换行)。 其他几个属性 pre 保留所有的格式符， pre-inline 合并空格符，保留换行符