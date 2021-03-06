# wechat-textinput

<p>示例演示：<br>
<img src="https://github.com/byg/wechat-textinput/raw/master/assets/images/demo.gif" alt="image" width='50%'>
</p>

用 wepy 开发自定义组件过程中发现页面加载自定义组件时能明显看到页面渲染的过程，组件越多表现的会越明显，于是用 mpvue 也写了个例子，发现 mpvue 没有这个问题，在这用代码记录下。下面是 wepy 版本和 mpvue 版本页面加载自定义组件 textinput 的三次对比

1. 9个组件对比，反复测试观察会发现 wepy 的页面能看到加载组件的过程，而不是给人一次性加载到页面上的感觉。

2. 为了明显对比效果，特意在 wepy 的页面 computed 里面加上背景色样式 `bg-scucess`，而 mpvue 的在 onLoad 方法里面也做同样设置

3. 更明显突出对比，在 wepy 和 mpvue 的 page 页面中引入多余的 components 组件，再次测试发现 wepy 的加载更慢了，而 mpvue 的页面没有变化。


### 调试发现

1. wepy 的组件如果样式直接写在 template 中是没有以上问题的，写在 data、computed 、onLoad 都会有问题；
2. wepy 写的页面 components 中声明的自定义组件越多，页面渲染的也会越慢；
3. wepy 的页面每次显示过程中都会依次的调用 onLoad ,调试时会发现组件是被一个一个加载到页面上的，不管是第一次还是第二次进入页面。而 mpvue 是第一次进入到这个页面时是一个一个加上去的，第二次进来虽然也走 onLoad，但页面是一次性显示加载完后的所有组件。

### 问题
1. wepy 中如果在初始化时样式是计算出来的或者是在其他地方控制的，会看到组件的渲染过程，页面越复杂时会越明显；
2. 页面引用自定义组件，官方文档有声明：
>WePY中的组件都是静态组件，是以组件ID作为唯一标识的，每一个ID都对应一个组件实例，当页面引入两个相同ID的组件时，这两个组件共用同一个实例与数据，当其中一个组件数据变化时，另外一个也会一起变化。
如果需要避免这个问题，则需要分配多个组件ID和实例

这样一来在页面中为一个组件声明多个 ID 的方式使得开发人员用起来比较麻烦，需要反复去写不同的 ID。

3. wepy 写的页面 components 中声明的自定义组件越多，页面渲染的也会越慢。
