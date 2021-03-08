<!--
 * @Description: 
 * @Version: 1.0.0
 * @Autor: yin gang
 * @Date: 2020-10-30 17:56:56
 * @LastEditors: yin gang
 * @LastEditTime: 2020-11-01 17:24:38
-->
### 生命周期
  * beforeCreated
      参数合并
      vue框架自身初始化  
        $children $parent $root $refs 
        $on $once $off $emit $forceUpdate $destroy 
        $slots $scopedSlots $createElement $nextTick
        做业务的时候很少在这个钩子里面写代码，一些vue的扩展插件比如vux vue-router 一般会在beforeCreated钩子里面进行插件的初始化.
  * created 
      数据初始化 包含data\props\methods\watch\computed\provide\inject 建立监听关系
  * beforeMount
      将模板编译成渲染函数
  * mounted
      虚拟dom已经转换为真实dom
  * beforeUpdate
  * updated
  * deactivated
  * activated
  * beforeDestroy
  * destroyed
  * errorCaptured
### 原理
    * 将template编译成render函数
    * 数据劫持
    * 第一次和空node进行patch，生成初始视图
    * 数据更新后，和上一状态vNode进行patch，更新dom
### props的运行原理，和data属性的不同

### 根vue应用和vue组件初始化的区别