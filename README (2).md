// import axios from 'axios'
// import Axios from 'axios'
// axios不需要使用Vue.use('axios')这个语法
// axios最常用的就是哪里需要就哪里引入进去
// 还有一种方法是在vue的原型链上注册axios
// Vue.prototype.$axios = axios
// 实验来看,在main.js和inedx.js注册都能使用,我认为只要在引用之前注册都可以
// 然后其他的组件就不需要import axios,而直接使用this.&axios.get就行
// 在header.vue组件中,script的type='text/ecmascript-6'ecma写成了ecam导致
// 无法识别es6语法而一直识别不出seller变量,找了将近一个小时
// 在header.vue组件的编写中,用了个v-if指令判断support是否存在,是因为
// seller数据是异步获取的,刚开始渲染的时候还不存在,所以要用if判断来执行,否则
// 取不到数据

//stylus 写法
@import '../../common/stylus/mixin.styl'//引入定义好的css函数的方式
bg-img($url)
  background-image:url($url + '@2x.png')
  @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
    background-image:url($url + '@3x.png')//定义css函数和媒体查询的方法

    .tab// 每向下一层缩进两格
      display:flex// 省略分号,冒号也可以省略
      width:100%// 寻找父元素用&替代
      height:40px// 子元素写在父元素属性里面
      line-height:40px// 引入stylus文件需要使用@开始
      border-1px(blue)// css设置细节参考header组件,其中vertical-align用的巧妙
      .tab-item
        flex:1
        text-align:center
        & > .links
          display: block
          bg-img('./brand')//使用stylus函数方法
          text-decoration:none
          font-size:14px
          color:rgb(77,85,93)
          &.active
            color: rgb(240,20,20)
// 两个inline-block或者inline元素之间会产生一点点空格,会影响排版,网上关于这个问题的解决方案的讨论也不少,难度不高,这里采用的是设置font-size为零的解决方式,副作用很大
// 把要用到的资源文件放在组件下,这样路径最小

//    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    }在header.vue中有使用这个钩子函数,我改成了直接使用函数形式的data,更直观

// 要使用图标字体文件,要在入口文件引入css文件或者stylus文件,比如index.js文件里面
css的filter属性是第一次用,貌似兼容性不是很好

图标字体的引入是个个例 注意和其他stylus引入的地方不一样

css sticky footer 方案汇总
sticky footer是指当内容不足够填充整个页面时候,footer要贴在底部,当内容超过页面时候,footer要在内容最下面
方法一:
总体思路:给页面设置两个div,第一个div放置内容,第二个div放置footer.然后第一个div在放置一个div,方便设置padding.
具体代码:
    .detail-wrap
      min-height:100%
      display:inline-block
      .detail-main
       // margin-top:64px
        padding-bottom:64px
    .detail-close
      // position:relative
     // width:32px
      //height:32px
      margin:-64px auto 0  auto
      //font-size:32px
// 在stylus文件夹下的全局css设置,除了mixin定义的函数在需要使用的组件里面需要额外引用之外,其他不需要

header组件开发总结:该组件功能并不复杂,父组件app.vue传了个数据,自己定义了两个数据,对于五种数据情况都是用数组来匹配,图标专用字体文件icon.styl.解决了几个css技巧,一个是1px线的生成,主要用了transform的screw功能,还有一个css sticky footer的问题,主要是html做文章,分两层,然后内容层设min-height为100%,footer层设一个负的margin-top,还有一个是flex自适应布局设置详情页的那两根翅膀线.根据不同分辨率的屏幕设置不同的图片,使用媒体查询并且封装了一个函数在mixin.stylus里面
css里面用了个&:last-child选择器,省了很多事,star子组件的评分系统实现很好,根据评分生成一个star的数组,里面的值不同,取相应的star图片.flex自适应布局是适合手机系统的.
header组件主要是css的编写,vue代码不多,用了一些指令和技巧,
//forEach()方法只遍历数组
//计算属性需要显示返回一个东西
/在goods组件中给左右布局的两个div各一个vue的特殊属性ref属性,作用是为了给元素或子组件注册引用信息,引用信息将会注册在父组件的 $refs 对象上。为了能够直接在父组件中访问子组件
//在编写cartcontrol组件时候,引入vue错误的写成了Vue,这么一个小小的错误找了好久,谨记
cartcontrol组件:
1,Vue.set( target, key, value )

参数：

{Object | Array} target
{string | number} key
{any} value
返回值： 设置的值.

用法：

设置对象的属性。如果对象是响应式的，确保属性被创建后也是响应式的，同时触发视图更新。这个方法主要用于避开 Vue 不能检测属性被添加的限制。

注意对象不能是 Vue 实例，或者 Vue 实例的根数据对象,
2, 购物车列表栏的切换用了一个fold和一个show的true和false分别来控制toggle,考虑了两种情况:有点单和没点单的情况,没点单,毫无疑问是fold为true,有点单,先设置fold为true,然后点击切换


//在vue中使用handler需要内置event参数时,要使用$在监听原生 DOM 事件时，方法以事件为唯一的参数。如果使用内联语句，语句可以访问一个 $event 属性： v-on:click="handle('ok', $event)"。

在父元素使用子元素定义的方法使用ref指令.
2,transition的使用参考food组件的使用方法,这是最基础的用法
3,提前给一个图片限制高度,站位的方法是设置padding:100%,具体参看food.vue img的css样式
4,组件的props选项的 数组/对象 的default应当由一个工厂函数返回,参见官网props指引
5,在ratingselect组件里面,为了能够使组件每次被调用的时候都能初始化,所以在show方法里面初始化写法
6,如果子元素使几个inlineblock的话,可以给父元素甚至font-size为0来消除inline-block之间的间隙,但是这个只适合小规模的,最好的还是把inline-block元素挨在一起.
7,子组件数据变化之后想要通知父组件使用emit方法.
8,FILTER方法需要数组
9,在food组件中,ref实现滚动的方式
10,子组件通过props获得的父组件的数据之后进行操作,数据的更新并不会影响父组件,所以一般都通过emit通知父组件数据更新,有个更巧妙的方法就是子组件触发事件之后并不更新数据,通知父组件自己更新
参见food组件和ratingselect组件的通信
11,时间戳用过滤器来实现.使用js文件模块化的方法
12,如果是export default的话,引入的时候不需要用花括号,否则要用花括号,同时可以引入多个
13,created是个函数,和methods等不一样
14,selectType的传导很有意思,绕了一圈,子元素获得改变之后直接传给父组件,父组件又通过props传回给子组件.
15,needShow这个用法值得记住

