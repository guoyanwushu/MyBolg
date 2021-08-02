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

* 一些解析url对象的方法，在参数值=0或者1的情况下，比如is_collect=0，解析出来的对象属性值是字符串 {is_collect: '0'}, 如果需要做布尔值的真假判断就需要注意一下 "0" 和 0 的区别

* 代码里面的无意义字符很大程度上会阻碍后续的人梳理清楚业务，特别是用在分支判断的时候，比如 type == 1 type == 2 这种，在接手的人不了解业务情况下根本就不知道这两个分支所对应的业务场景，一个解决方法是用有意义的常量来代替无意义的数值，比如 type == PAY_SUCCESS type = PAY_FAIL type = PAY_ABORT ，一定程度上可以方便区分，如果场景和因素比较多，不方便用常量描述，则有必要用注释来进行说明。

* try catch的时候，如果在catch里面又throw了错误, 比如 throw new TypeError('is not a variable'), 还是会中断后续程序运行的。所以为了不中断，catch里面就不要再抛出错误了。

* 安全的判断一个变量是否已定义用 typeof

* 在用v-html时发现的问题，如果v-html的值里面包含有格式符号比如 \n \t \r ,不处理的情况，v-html渲染出来是没有格式的。如果需要有格式，则需要给v-html的元素加上 white-space: pre-wrap (保留空白符，正常换行)。 其他几个属性 pre 保留所有的格式符， pre-inline 合并空格符，保留换行符

* 滚动条的光标问题，滚动条的光标一般会继承滚动内容区域的光标类型。如果在滚动区域的光标类型是箭头，那么滚动条的光标类型就是箭头；如果滚动区域是textarea，在不特定设置光标类型的情况下，textarea区域的光标类型是文字光标，那么当textarea出现滚动条的时候滚动条的光标就是文字。 如果在这种情况下，要保持滚动条的光标为箭头，那么需要为textarea区域设置样式cursor: auto, 这个时候光标就是对的了。

* td的min-height style属性在chrome、火狐等现代浏览器均不生效，但是在ie里面却有效，就很神奇

* 在页面里可以通过window.print()来调用打印机打印当前页面， 默认是打印整个html，但是可以通过 @media print { #header {display: none}} 来屏蔽掉在打印时不需要显示的部分，另外打印的尺寸是看页面的内容分布尺寸，margin部分是不会打印的

* 关于textarea的高度跟随输入文字自适应的问题, 两个思路，一个方法是利用textarea的oninput或者onpropertychange属性，在输入时动态将textarea的scrollHeight属性赋值给height或者posHeight(该属性在chrome下不生效);另外一个思路是在textarea获取焦点的时候，开启一个循环定时器来将scrollHeight赋值给height，然后在textarea失去焦点的时候清除掉计时器

* 关于移动端缩放和定位的问题，需要在移动端缩放后的区域内定位的时候，有时候通过offset。top这种和其他元素对比得出的定位距离不一定准，需要除以对应的缩放系数，才能在有缩放区域内定到正确的位置

* 关于在textarea中，进行中文输入的情况下，手动输入4个空格顶行的操作，在PC端输入状态4个空格刚刚和2个中文汉字对齐，没有问题，但是回显的时候，4个空格和两个中文汉字没有对齐，比两个中文汉字还宽了一点点。经查阅是不同浏览器对键盘空格录入的空格("\&nbsp;\")解析宽度不一样，所以就会有ie对不齐但是谷歌能对齐的情况。解决方案有三种，一种是在键入空格的时候，输入全角状态下的中文空格; 另外一种是在回显的时候，将"\&nbsp;\"替换为"\&ensp;\"还有就是将字体设置为中英文等宽字体,比如宋体，这样子也会对齐；那些非中英文等宽字体就不一定对得齐。

* 上面问题的引申问题， 考虑这样子的场景。在PC端录入一些信息的时候，假如录入了10个空格，此时在pc端空格是呈现了一定的距离的，数据库录入的就是对应个数的&nbsp;然后在移动端回显的时候，就会有问题。在pc端用&nbsp;占出来的距离，在移动端变短了;原因是在高分屏比如三倍屏，3个空格才能达到pc端1个空格的距离，所以就变短了。目前有两个解决方案，一个是根据高分屏的倍率，在呈现的时候将1个空格替换为三个空格，另外一个就是将&nbsp;替换为&emsp;自适应中文空格就行了。

* event对象的相关属性着重记忆一下
  event.clientX  事件发生点离浏览器视口的距离
  event.offsetX  事件发生点离其最近的有定位属性的父元素的距离, 也有可能是元素自身(比如drop事件，可能拖拽的距离很短，拖拽还是落在了元素自己身上，其次才会冒泡到外层的容器)
  event.pageX    事件发生点离文档左上角的距离，因为文档过长存在滚动关系，所以pageX一般是大于等于clientX的
## 关于跨域的身份认证识别
> 在ｗebpack中通过devServer的proxy的代理设置，将接口代理规避跨域但是针对有些后台接口需要认证信息才能返回数据;依稀记得之前的解决方案是在axios里面发起请求前添加请求头
{Cookie: 'JESSIONID=12324556'}这种；但是昨天试了一下居然不行了，浏览器会报Refused to set unsafe header Cookie,错误，然后身份信息怎么也带不过去，后来兵哥给了另外一种方
案,如下在devServer代理里面添加Cookie，实现了身份认证，就狠神奇 

    module.exports = {
      proxy: {
          '/seeyon': {
              target: proxySite,
              changeOrigin: true,
              pathRewrite: {
                  '^/seeyon': '/seeyon'
              },
              headers: {
                  Cookie: 'JSESSIONID=BD1394C2A5D6CB8242E3DC852771AAD9'
              }
          }
      },
      port: 8000
  }    

  * npm安装依赖的时候， 有些项目依赖分为外部依赖和内部依赖，针对这种需要切换npm源的情况，解决方案暂时了解的有如下几种
    1. 通过npm config set registry xxxxx 手动切换源来进行安装；缺点就是比较麻烦
    2. 通过在scripts里配置安装命令:upb: npm --registry http://192.168.225.99:4837/ uninstall cap4-business -S && npm --registry http://192.168.225.99:4837/ install cap4-business -S这种临时设置源的方法不会对默认源进行修改，安装的时候直接 npm run upb 就可以安装了，比刚刚说的要方便一些，但是要完整安装完依赖还是需要 npm i; npm run upb ; npm run seeyon-ant 这样子执行几次命令的
    3. 通过配置.nrmrc 文件, 在npmrc中配置一些依赖库的下载路径，避免因为网络问题出现某些依赖下载失败(node-sass说的就是你)还是很有用的
        ```javascript
        registry=https://registry.npm.taobao.org/
        sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
        electron_mirror=https://npm.taobao.org/mirrors/electron/
        phantomjs_cdnurl=https://npm.taobao.org/mirrors/phantomjs/
        ```

*  动态插入到head中js的执行顺序
      ```javascript
    var script = document.createElement('script');
    script.src = 'xxxx'
    document.getElementsByTagName('head')[0].appendChild(script);
      ```
    类似于上诉的动态插入的脚本，在执行上有几个地方要注意一下。    
    1. 插入的js默认是async = true，谁先下载完谁执行, 所以js的执行顺序并不能保证和插入顺序一致，如果要保证执行顺序和插入顺序一致，那么设置script.async = false, 可以保证动态插入的脚本按插入顺序执行
    2. 动态插入的脚本不会阻塞浏览器渲染和后面的脚本执行，所以后面的脚本执行的时候，动态插入的脚本有可能执行完了有可能没执行完；如果后续的脚本依赖了动态插入的脚本的执行结果，极有可能会出错    

    解决方案：
    有没有觉得和amd模块方案是不是有点类似? require(['a.js', 'b.js', 'c.js'], function(a,b,c) {}); 不同的是我们的需求更为简单，只需要同步执行就行了


* script内部报错的问题    
也是比较有意思的问题。html中的一段script代码,前面声明了一些变量，然后有个语法错误，然后会导致整段script执行失败，并且在出错代码之前声明的变量也会没了。    

* 关于babel转换语法的问题，在vue-cli3默认情况下，src下面的业务代码都是转了的，但是node_modules里面的三方库文件，不一定是转了的，所以就会造成有些诸如const、let之类的变量在ie下面直接就语法错误了，这个时候就需要把需要的三方库也用babel转一哈    
在vue.config.js的导出对象中加一行
  ``` javascript
    transpileDependencies: ['./src', /[/\\]node_modules[/\\]seeyon-ui-ant[/\\]/],
  ```

* ie的字体图标问题
任信的问题，字体图标在chrome下面显示正常，但是在ie下面始终无法显示。后经过排查是加载的字体文件的问题，ie加载的是eot格式的字体图标(eot是ie的专用字体格式，其他浏览器不支持eot)，恰好大研发那边没有再维护eot字体文件了，所以没有那两个图标，所以导致在ie里面始终无法加载出对应的字体图标。由于ie9已经支持woff格式的字体图标了，所以如果没有ie9以下的支持需求的话，干脆把css里面的eot引用删了。

* 离开浏览器时的执行操作
```javascript
 window.onbeforeunload = function (e) {
    e = e || window.event;
    // 兼容IE8和Firefox 4之前的版本
    if (e) {
        e.returnValue = '关闭提示';
    }
    // Chrome, Safari, Firefox 4+, Opera 12+ , IE 9+
    return '关闭提示';
 };
```
注意, 通过文件路径直接访问并关闭页面亲测是不会触发的，只有通过http打开 localhost 或者 ip，才会触发





  