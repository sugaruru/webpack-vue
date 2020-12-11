# Vue.js

## Node(后端)中的 MVC 与 前端中的 MVVM 之间的区别

+ MVC 是后端的分层开发概念；
+ MVVM 是前端视图层的概念， 主要关注与 视图层分离， 即：MVVM把前端视图层分成三部分 Model, View, VM ViewModel

## Vue.js 基本代码 和 MVVM 之间的对应关系
+ V层就是视图层，对应的关系是body 里面的#app里面需要渲染显示的。
+ new Vue的实例就是VM调度者
+ 而M 就是data，保存每个页面的数据

## Vue.js 的基本代码结构，v-cloak， v-text
### 基本代码结构
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


### 使用 v-cloak 能够解决 插值表达式闪烁的问题 
### 默认 v-text 是没有闪烁问题的 
    v-text会覆盖元素中原本的内容，但是 插值表达式  只会替换自己的这个占位符，不会把整个元素的内容清空

## Vue 指令之 `v-bind` 的用法(Vue提供的属性绑定机制)
1. 直接使用指令 `v-bind`
2. 使用简化指令 `:`
3. 在绑定的时候，拼接绑定内容：` :title="btnTitle + ', 这是追加的内容'" `

## Vue的`v-on` 事件绑定机制 简写是` @ `
### 跑马灯案例
1. HTML结构 
```
<div id="app">

    <p>{{info}}</p>

    <input type="button" value="开启" v-on:click="go">

    <input type="button" value="停止" v-on:click="stop">

  </div>

```
2. Vue实例：
```
	// 创建 Vue 实例，得到 ViewModel

    var vm = new Vue({

      el: '#app',

      data: {

        info: '猥琐发育，别浪~！',

        intervalId: null

      },

      methods: {

        go() {

          // 如果当前有定时器在运行，则直接return

          if (this.intervalId != null) {

            return;

          }

          // 开始定时器

          this.intervalId = setInterval(() => {

            this.info = this.info.substring(1) + this.info.substring(0, 1);

          }, 500);

        },

        stop() {

          clearInterval(this.intervalId);

        }

      }

    });

```
### v-on 事件修饰符
+ .stop     阻止冒泡
+ .prevent  阻止默认事件
+ .capture  添加事件侦听器时使用事件捕获模式
+ .self     只当事件在该元素（比如不是子元素）触发是触发回调
+ .once     事件只触发一次
  
## Vue指令之`v-model`和`双向数据绑定`
### 简易计算器案例
1. HTML 代码结构
```
  <div id="app">
    <input type="text" v-model="n1">
    <select v-model="opt">
      <option value="0">+</option>
      <option value="1">-</option>
      <option value="2">*</option>
      <option value="3">÷</option>
    </select>
    <input type="text" v-model="n2">
    <input type="button" value="=" v-on:click="getResult">
    <input type="text" v-model="result">
  </div>
```
2. Vue实例代码：
```
	// 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        n1: 0,
        n2: 0,
        result: 0,
        opt: '0'
      },
      methods: {
        getResult() {
          switch (this.opt) {
            case '0':
              this.result = parseInt(this.n1) + parseInt(this.n2);
              break;
            case '1':
              this.result = parseInt(this.n1) - parseInt(this.n2);
              break;
            case '2':
              this.result = parseInt(this.n1) * parseInt(this.n2);
              break;
            case '3':
              this.result = parseInt(this.n1) / parseInt(this.n2);
              break;
          }
        }
      }
    });
```
### Vue中v-model使用
1. 在原生表单元素中
```
<input v-model="inputValue">
// 相当于
<input v-bind:value="inputValue" v-on:input="inputValue=$event.target.value">
```
2. 在自定义组件中
```
<my-component v-model="inputValue"></my-component>
// 相当于
<my-component v-bind:value="inputValue" v-on:input="inputValue = argument[0]"></my-component>
```

## 在Vue中使用样式
### 使用class样式
1. 数组
```
<h1 :class="['red', 'thin']">这是一个邪恶的H1</h1>
```
2. 数组中使用三元表达式
```
<h1 :class="['red', 'thin', isactive?'active':'']">这是一个邪恶的H1</h1>
```
3. 数组中嵌套对象
```
<h1 :class="['red', 'thin', {'active': isactive}]">这是一个邪恶的H1</h1>
```
4. 直接使用对象
```
<h1 :class="{red:true, italic:true, active:true, thin:true}">这是一个邪恶的H1</h1>
```

### 使用内联样式
1. 直接在元素上通过 `:style` 的形式，书写样式对象
```
<h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>
```
2. 将样式对象，定义到 `data` 中，并直接引用到 `:style` 中
 + 在data上定义样式：
```
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' }
}
```
 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```
<h1 :style="h1StyleObj">这是一个善良的H1</h1>
```
3. 在 `:style` 中通过数组，引用多个 `data` 上的样式对象
 + 在data上定义样式：
```
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' },
        h1StyleObj2: { fontStyle: 'italic' }
}
```
 + 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```
<h1 :style="[h1StyleObj, h1StyleObj2]">这是一个善良的H1</h1>
```

## Vue指令之`v-for`和`key`属性
1. 迭代数组
```
<ul>
  <li v-for="(item, i) in list">--索引值--{{i}}--每一项--{{item}}</li>
</ul>
```
2. 迭代对象数组
```
// 在data中定义对象数组
data:{
      list:[1,2,3,4,5,6],
      listObj:[
        {id:1, name:'zs1'},
        {id:2, name:'zs2'},
        {id:3, name:'zs3'},
        {id:4, name:'zs4'},
        {id:5, name:'zs5'},
        {id:6, name:'zs6'},
      ]
}
// 在html中使用 v-for 指令渲染
<p v-for="(user,i) in listObj">--id--{{user.id}}   --姓名--{{user.name}}</p>
```
3. 迭代对象
```
// 在data中定义对象
data:{
      user:{
        id:1,
        name:'托尼.贾',
        gender:'男'
      }
}
// 在html中使用 v-for 指令渲染
<p v-for="(val,key,i) in user">--键是--{{key}}--值是--{{val}}</p>
```
4. 4.迭代数字
```
<!-- 注意：如果使用v-for迭代数字的话，前面 count 的值从 1 开始-->
<p v-for="count in 10">这是第{{count}}个p标签</p>
```
### v-for的key

2.2.0+ 的版本里，**当在组件中使用** v-for 时，key 现在是必须的。

当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。

为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。

## Vue指令之 `v-if ` 和 ` v-show `
> 一般来说， v-if 有更高的切换消耗， v-show有更高的初始渲染消耗。因此，如果需要频繁切换 v-show 较好， 如果在运行时条件不大可能改变 v-if较好。

## 过滤器
过滤器可以用在两个地方
- 双花括号插值
- v-bind 表达式
过滤器应该被添加到JavaScript表达式的尾部，由“管道”符表示

### 私有过滤器
1. HTML元素：
```
<td>{{item.ctime | dataFormat('yyyy-mm-dd')}}</td>
```
2. 私有 `filters` 定义方式：
```
filters: { // 私有局部过滤器，只能在 当前 VM 对象所控制的 View 区域进行使用
    dataFormat(input, pattern = "") { // 在参数列表中 通过 pattern="" 来指定形参默认值，防止报错
      var dt = new Date(input);
      // 获取年月日
      var y = dt.getFullYear();
      var m = (dt.getMonth() + 1).toString().padStart(2, '0');
      var d = dt.getDate().toString().padStart(2, '0');
      // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日
      // 否则，就返回  年-月-日 时：分：秒
      if (pattern.toLowerCase() === 'yyyy-mm-dd') {
        return `${y}-${m}-${d}`;
      } else {
        // 获取时分秒
        var hh = dt.getHours().toString().padStart(2, '0');
        var mm = dt.getMinutes().toString().padStart(2, '0');
        var ss = dt.getSeconds().toString().padStart(2, '0');
        return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;
      }
    }
  }
```
> 使用ES6中的字符串新方法 String.prototype.padStart(maxLength, fillString='') 或 String.prototype.padEnd(maxLength, fillString='')来填充字符串；

### 全局过滤器
```
// 在创建Vue实例之前，定义一个全局过滤器
Vue.filter('dataFormat', function (input, pattern = '') {
  var dt = new Date(input);
  // 获取年月日
  var y = dt.getFullYear();
  var m = (dt.getMonth() + 1).toString().padStart(2, '0');
  var d = dt.getDate().toString().padStart(2, '0');
  // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日
  // 否则，就返回  年-月-日 时：分：秒
  if (pattern.toLowerCase() === 'yyyy-mm-dd') {
    return `${y}-${m}-${d}`;
  } else {
    // 获取时分秒
    var hh = dt.getHours().toString().padStart(2, '0');
    var mm = dt.getMinutes().toString().padStart(2, '0');
    var ss = dt.getSeconds().toString().padStart(2, '0');
    return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;
  }
});
```
> 注意：当有局部和全局两个名称相同的过滤器时候，会以就近原则进行调用，即：局部过滤器优先于全局过滤器被调用！

## vue实例的生命周期
+ 什么是生命周期：从Vue实例创建，运行，到销毁时间，总是伴随着各种各样的事件，这些事件，统称为生命周期
+ 生命周期钩子 = 生命周期函数 = 生命周期事件
+ 主要的生命周期函数分类：
  - 创建期间的生命周期函数：
    + beforeCreate：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
    + created: 实例已经在内存中创建ok， 此时 data 和 methods 已经创建ok， 此时还没有开始 编译模板
    + beforeMount：此时已经完成了模板的编译，但是还没有挂载到页面中去
    + mounted：此时，已经将编译好的模板，挂载到了页面指定的容器中显示
  - 运行期间的生命周期函数
    + beforeUpdate：状态更新之前执行此函数， 此时 data 中的状态值是最新的， 但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染DOM节点
    + updated：实例更新完毕之后调用此函数， 此时 data中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！
  - 销毁期间的生命周期函数：
    + beforeDestroy：实例销毁之前调用。在这一步，实例任然完全可用
    + destroyed：Vue实例销毁后调用。调用后，Vue实例指示的所有东西都会解绑定， 所有的事件监听器会被移除，所有的子实例也会被销毁。

![生命周期](./images/lifecycle.png)

## [vue-resource 实现 get, post, jsonp请求]
vue-resource是vue中使用请求网络数据的插件， 这个插件是依赖于vue的，简单说就是用来调接口的
- 安装
  ```
  cd 项目目录
  npm i vue vue-resource --save-dev
  ```
- 在入口文件main.js中配置
```
import Vue from 'vue'
import VueResource from 'vue-resource'
Vue.use(VueResource)
```

```
// 基于全局Vue对象使用http
Vue.http.get('/someUrl', [options]).then(successCallback, errorCallback);
Vue.http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);

// 在一个Vue实例内使用$http
this.$http.get('/someUrl', [options]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
```


- get 请求
```
  this.$http.get('地址',{params: {参数}}).then(function(res) {
        console.log(res)
        // 响应成功回调
    },function(res) {
        console.log(res)
        // 响应错误回调
    })   
```
- post请求：
```
this.$http.post('地址',{参数},{emulateJSON:true}).then( function(res) {
      console.log(res.data)
    })
```
- jsonp请求：
```
this.$http.jsonp("地址",{params:{参数}}).then( function(res){
      console.log(res.data.s)
    })  
```   
  
## 配置本地数据库和数据接口API(??没做)
1. 先解压安装 `PHPStudy`;
2. 解压安装 `Navicat` 这个数据库可视化工具，并激活；
3. 打开 `Navicat` 工具，新建空白数据库，名为 `dtcmsdb4`;
4. 双击新建的数据库，连接上这个空白数据库，在新建的数据库上`右键` -> `运行SQL文件`，选择并执行 `dtcmsdb4.sql` 这个数据库脚本文件；如果执行不报错，则数据库导入完成；
5. 进入文件夹 `vuecms3_nodejsapi` 内部，执行 `npm i` 安装所有的依赖项；
6. 先确保本机安装了 `nodemon`, 没有安装，则运行 `npm i nodemon -g` 进行全局安装，安装完毕后，进入到 `vuecms3_nodejsapi`目录 -> `src`目录 -> 双击运行 `start.bat`
7. 如果API启动失败，请检查 PHPStudy 是否正常开启，同时，检查 `app.js` 中第 `14行` 中数据库连接配置字符串是否正确；PHPStudy 中默认的 用户名是root，默认的密码也是root