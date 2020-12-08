Vue.js

# Node(后端)中的 MVC 与 前端中的 MVVM 之间的区别

+ MVC 是后端的分层开发概念；
+ MVVM 是前端视图层的概念， 主要关注与 视图层分离， 即：MVVM把前端视图层分成三部分 Model, View, VM ViewModel

# Vue.js 基本代码 和 MVVM 之间的对应关系
+ V层就是视图层，对应的关系是body 里面的#app里面需要渲染显示的。
+ new Vue的实例就是VM调度者
+ 而M 就是data，保存每个页面的数据

# Vue.js 的基本代码结构，v-cloak， v-text
## 基本代码结构
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
        <!-- 1. 导入Vue的包 -->
        <script src="./lib/vue-2.4.0.js"></script>
    </head>
    <body>
        <!-- 将来 new 的Vue实例，会控制这个 元素中的所有内容 -->
        <!-- Vue 实例所控制的这个元素区域，就是我们的 V  -->
        <div id="app">
            <p>{{ msg }}</p>
        </div>

        <script>
            // 2. 创建一个Vue的实例
            // 当我们导入包之后，在浏览器的内存中，就多了一个 Vue 构造函数
            //  注意：我们 new 出来的这个 vm 对象，就是我们 MVVM中的 VM调度者
            var vm = new Vue({
                el: '#app',  // 表示，当前我们 new 的这个 Vue 实例，要控制页面上的哪个区域
                // 这里的 data 就是 MVVM中的 M，专门用来保存 每个页面的数据的
                data: { // data 属性中，存放的是 el 中要用到的数据
                    msg: '欢迎学习Vue' // 通过 Vue 提供的指令，很方便的就能把数据渲染到页面上，程序员不再手动操作DOM元素了【前端的Vue之类的框架，不提倡我们去手动操作DOM元素了】
                }
            })
        </script>
    </body>
</html> 

## <!-- 使用 v-cloak 能够解决 插值表达式闪烁的问题 -->
## <!-- 默认 v-text 是没有闪烁问题的 --> 
    v-text会覆盖元素中原本的内容，但是 插值表达式  只会替换自己的这个占位符，不会把整个元素的内容清空

    test