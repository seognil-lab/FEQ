“每18至24个月，前端都会难一倍”
——《前端服务化之路》赫门, 2015深JS

js部分我司大量使用jQuery，答题时请随意使用

# 简答
## 一 基础

1.1 盒子模型（Block Box）除了margin还有哪些结构，他们有层级关系吗？
1.2 你使用或了解过Postion的哪些值，使用上有什么需要注意的地方？
1.3 你使用或了解过Display的哪些值，他们有什么作用？
1.4 用哪些方法可以使元素视觉上不可见？
1.5 外边距折叠（外边距合并）是什么意思，举例说明在什么情况下会出现这个效果，如何避免？
1.6 css中:before和::before有什么区别？


1.1
border
padding content（内容）
background-image
background-color
margin

[CSS(盒模型,布局模型)](http://www.jianshu.com/p/1094c84c2f17)

1.2
常规流
    static
    relative
Float
    float
绝对定位
    absolute
    fixed
    sticky（CSS3新属性）

float可以实现横排效果，使用完后需要用clear清除浮动
float和绝对定位会脱离常规流
改变定位会产生回流导致页面重绘


[视觉格式化模型](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Visual_formatting_model)
[页面重绘和回流以及优化](http://www.css88.com/archives/4996)


1.3
block 块级上下文
inline 行内上下文
flex 弹性盒子布局
grid 网格布局

none 隐藏
inline-block 行内块
table等

[小科普：到底什么是 BFC、IFC、GFC 和 FFC](https://juejin.im/entry/5938daf7a0bb9f006b2295db)


1.4
display: none;
opacity: 0;
visibility: hidden;

$.fadeOut()
$.remove()

transform: scale(0);
position: fixed; top: -999px;
height: 0; overflow: hidden;
z-index: -999999;

[您可能不知道的CSS元素隐藏“失效”以其妙用](http://www.zhangxinxu.com/wordpress/2012/02/css-overflow-hidden-visibility-hidden-disabled-use/)
[小tips: zoom和transform:scale的区别](http://www.zhangxinxu.com/wordpress/2015/11/zoom-transform-scale-diff/)


1.5
相邻的盒子垂直外边距会被折叠（相邻是指没有padding、border等隔开或一些其他约束）

（同级正margin）
    两个相邻元素都有正margin，这两个元素间的实际margin是两个margin的最大值
（父子正margin）
    父元素和子元素都有正margin，这两个元素叠加形成的实际margin是两个margin的最大值

[margin系列之外边距折叠](http://blog.doyoe.com/2013/12/04/css/margin%E7%B3%BB%E5%88%97%E4%B9%8B%E5%A4%96%E8%BE%B9%E8%B7%9D%E6%8A%98%E5%8F%A0/)


1.6
一个冒号，伪类，如:first-child，表示当前选择器中，符合这个约束的某些元素，（逻辑上可视为filter）
两个冒号，伪元素，如::before，（逻辑上可视为child）

双号冒号是CSS3中的定义
大部分伪元素可以使用一个冒号，现代化浏览器也可以正常解析
两个冒号，旧版本IE不支持解析

[:before和::before的区别](https://www.qianduan.net/before-and-before-the-difference-between/)



2.1 js有哪些数据类型，他们可以分成哪两类？有什么区别？
2.2 NaN、null、undefined、0、false，在代码执行时有什么异同？
2.3 var arr = new Array([3,4])，如何判断arr的类型？
2.4 如何复制（克隆）一个数组？
2.5 以arr为例，__proto__和prototype有什么区别

2.1
原始类型（值类型）
    undefined
    null
    Boolean
    Number
    String
    Symbol（ES6 新标准）
对象类型（引用类型）
    Object

Array、Function等是js的预设类型，本质都是Object，继承于Obejct

[JavaScript核心概念归纳整理](https://mp.weixin.qq.com/s/I7A1iC8Et6uOGZ234DsTlA)


2.2
都可直接用于判断为false（隐式类型转换）

NaN和0是数字类型，false是Boolen类型，
null就是null
undefined就是undefined
null 可视为缺少对象（可用于清除变量，指定垃圾回收）
undefined 可视为缺少值（可用于清除对象的值）


[JavaScript 内存泄漏教程](http://www.ruanyifeng.com/blog/2017/04/memory-leak.html)

2.3
typeof，typeof arr === "object"
Array.isArray(arr) === true
instanceof，arr instanceof Array === true
Object.prototype.toString.call(arr).split(/[\[\s\]]/)[2] === "Array";
jQuery，$.isArray(arr) === true
lodash，_.isArray(arr) === true


2.4
直接等号赋值是错误方法，只是原变量的引用，修改新变量会同时改变原变量

手动for循环
arr.slice()
Array.prototype.slice.call(arr) （类数组对象也能用）
Object.assign([],arr) （所有对象变量都能使用，返回值引用了第一个参数）

$.extend([],arr)
_.merge([],arr)

2.5
__proto__，隐式原型
prototype，显式原型
一个对象的__proto__是其构造函数的prototype


arr.__proto__ === Array.prototype
arr.__proto__.__proto__ === Array.prototype.__proto__ === Object.prototype

prototype可用于声明自定义方法
Array.prototype.slice.call()

[从__proto__和prototype来深入理解JS对象和原型链](https://github.com/creeperyang/blog/issues/9)



## 工程


3.1 开发时，如何在快速书写以下HTML结构？
3.2 页面加载后，如何在页面中动态生成以下HTML结构？

<div class="outerWrapper">
    <div class="innerInfo">
        <div class="infoImg"></div>
        <div class="infoTitle"></div>
        <div class="infoDesc"></div>
    </div>
    <div class="innerPanel">
        <div class="innerBtn"></div>
        <div class="innerBtn"></div>
        <div class="innerBtn"></div>
    </div>
</div>


3.1
手写（手动复制）
快捷键复制行和多行编辑
Emmet
模板语言（JADE）预编译

3.2
页面加载后
    window.addEventListener('load', fn, false )
    jQuery ready
    其他框架语法
jQuery实现
原生js实现
使用框架 组件化


3.3 有哪些方法能减小图片对网页加载的影响（只需提及大致方法）

3.3
雪碧图（合并请求）
懒加载（优先渲染其他部分）
预处理（减小文件尺寸）
转译成Base64编码，使用DataURL引用
css、SVG、canvas化

3.4 目前有哪些浏览器引擎。从这个角度来看，开发中我们可能碰到哪些相关问题？如何解决？

3.4
渲染引擎
    Webkit——Safari、Chrome、（Opera）
    Gecko——Firefox
    Trident——IE
JS引擎
    V8——Chorme
    JSCore——Safari
    SpiderMonkey——FireFox
    Chakra——新版IE（IE9+）


引擎的各自实现有些超前于标准，有些落后于标准
渲染引擎主要影响显示效果，不同渲染引擎渲染方式不一样，可能同一份代码在不同平台上有不同效果，
同一款浏览器的旧版本可能不支持一些新特性

可能各自会有不同的私有属性方法
有的私有属性可能无法在所有平台实现，
比如backdrop-filter: blur()，只能在safari上实现
比如theme-color只能在Android的chrome上实现

CSS兼容性
    手动写前缀
    预处理语言mixin
    autoprefixer watch编译


同理，JS新语法标准，不同的JS引擎可能实现覆盖程度不同，旧版本的浏览器有些新特性无法正确解析
    如果使用新语法，可以使用babel转义到旧语法，解决兼容性问题

查看兼容性
http://caniuse.com/
https://kangax.github.io/compat-table/es6/


3.5 操控元素的简单动画效果可能有哪些方法
3.6 如果需要手动逐帧绘制以实现复杂效果呢（比如抛物线），该如何实现？
3.7 setInterval的最小间隔设为多少比较合理？

触发
    纯css（hover等）
    js toggleClass或类似方法
    js直接修改元素css
    js修改stylesheet内容
动画实现
    直接修改元素css属性
    css，animation
    Animate.css等动画库
    jQuery，animate()、css()等


多层wrapper叠加
    抛物线本身可以用css两层wrapper实现（用cubic-bezier分别控制水平和垂直速度）

setInterval
requestAnimationFrame


16.7ms（1000毫秒/屏幕刷新率60Hz）
过小造成浪费，过大产生卡顿



# 读代码

解释以下代码，并说明可能涉及哪些知识点

.iFilterWrapper > .iFilterResult {
  margin: 4px 2px;
  color: #039;
  padding: 0 1em;
  border-radius: 2px;
  cursor: pointer;
  -webkit-box-flex: 1;
      -ms-flex-positive: 1;
          flex-grow: 1;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
}


尺寸单位（px em）
0可以省略单位
属性缩写（margin color padding）
border-radius（CSS3属性）
cursor（指针）
css选择器（直接子代选择器）
flex布局
css优先级（多个display）
私有前缀（ms webkit 兼容性）
预编译（autoprefixer 或 mixin）
代码格式化（缩进、对齐等）

.iFilterWrapper {
    & > .iFilterResult {
        flex-grow: 1;
        display: flex;
        align-items: center;
    }
}

###

解释以下代码，并说明可能涉及哪些知识点

let $j3 = jQuery.noConflict(true)
let $j2 = jQuery.noConflict(true)

(function ($) {
    $(function () {
        initPage($)
    });
})($j3);

function initPage($) {
    renderBanner()
    addBannerEvent()

    function renderBanner() {
        var bannerGroup = [{
            "title": "活动1",
            "img": "event1.png",
            "url": "/event1",
            "id": "event1",
        }, ]
        renderBannerTemp(bannerGroup) // 自定义函数，这里未写出
    }

    function addBannerEvent() {
        var $bannerItem = $(".bannerWrapper").find(".bannerItem")
        $.each($bannerItem, function (i, ele) {
            var $this = $(ele);
            $this.on("click", function () {
                urlJump($this.data("url")) // 自定义函数，这里未写出
            })
        })
    }
}



自执行匿名函数和参数调用
jQuery防止冲突
jQuery document ready
闭包（initPage）
函数提升（initPage renderBanner addBannerEvent）
小函数
缓存jQuery选择器结果以提升性能
命名习惯（$bannerItem）
jQuery方法（选择器 each on）
大部分分号可省略
尾逗号可正常解析（从ES5开始支持）

[[译] JavaScript：立即执行函数表达式（IIFE）](https://segmentfault.com/a/1190000003985390)
[在同一个页面使用多个不同的jQuery版本而不冲突的方法](http://125196721-qq-com.iteye.com/blog/1434413)
[jQuery事件详解之$(document).ready()](http://www.jianshu.com/p/ac82ffa44bcb)
[学习Javascript闭包（Closure）](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)
[JavaScript 中的变量和函数提升](https://jinlong.github.io/2013/09/11/var-and-fun-hoisting/)
[The art of writing small and plain functions](https://rainsoft.io/the-art-of-writing-small-and-plain-functions/?utm_source=wanqu.co&utm_campaign=Wanqu+Daily&utm_medium=website)
[jQuery最佳实践](http://keenwon.com/955.html)
[JavaScript 语句后应该加分号么？](https://www.zhihu.com/question/20298345)
[Airbnb JavaScript 代码规范（ES6）](https://www.kancloud.cn/kancloud/javascript-style-guide/43125)



###

用ES4方法实现ES6模板字符串（只填充数据），fillText函数内大致有哪些步骤（只需说明步骤）

var infoTemp = '${candidate}你好，您的${jobTitle}职位已经申请${statusText}。'
var jobApply = {
    candidate: '张三',
    jobTitle: '前端工程师',
    statusText: "成功",
}
console.log(fillText(infoTemp, jobApply)) // 张三你好，您的前端工程师职位已经申请成功。


replace
正则
全局搜索（g）
捕获（圆括号）


视野：知道ES4和ES6的区别
基础：字符串replace
基础：正则
基础：正则全局搜索
工程：对undefined进行标记以方便排错

function fillText(e, d) {
    return e.replace(/\$\{([^\{^\}]*)\}/g, function (match, r) {
        return d[r] || match + "?"
    })
}

[正则表达式案例分析 （二）](https://juejin.im/post/5973f16c51882573c14a812d?utm_source=gold_browser_extension)
[深入解析 ES6：模板字符串](http://bubkoo.com/2015/06/23/es6-in-depth-template-strings/)
