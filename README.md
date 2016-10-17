# vue-drapload

这是基于vue的一个下拉刷新和加载更多的插件。
使用手机打开。或者用chrome切换成手机模拟器查看效果。

[在线例子-基础版](http://htmlpreview.github.io/?https://github.com/jy03078959/vue-drapload/blob/master/example/base.html)

[在线例子-自定义版](http://htmlpreview.github.io/?https://github.com/jy03078959/vue-drapload/blob/master/example/config.html)




# 安装//Install

```npm
npm install vue-drapload --save
```

###ES6

```JavaScript
import vueDrapload from 'vue-drapload'
Vue.use(vueDrapload,config)
```

###CommonJS

```JavaScript
var vueDrapload =  require('vue-drapload');
Vue.use(vueDrapload,config)
```

###直接引用//Direct include

```JavaScript
<script src="../node_modules/vue_scroll/vue-drapload.js"></script>
```
### 引入默认加载效果的css
```css
.dropload-up {
      position: relative;
      height: 0;
      overflow: hidden;
      font-size: 12px;
      -webkit-transform: translateZ(0);
      transform: translateZ(0);
    }
    .dropload-refresh, .dropload-update, .dropload-load, .dropload-noData {
      height: 50px;
      line-height: 50px;
      text-align: center;
    }
    .dropload-load .loading {
      display: inline-block;
      height: 15px;
      width: 15px;
      border-radius: 100%;
      margin: 6px;
      border: 2px solid #666;
      border-bottom-color: transparent;
      vertical-align: middle;
      -webkit-animation: rotate 0.75s linear infinite;
      animation: rotate 0.75s linear infinite;
    }
    @-webkit-keyframes rotate {
      from {-webkit-transform:rotate(0deg);}
      to {-webkit-transform:rotate(360deg);}
    }
```



# 使用方法//Usage

Use v-scroll to enable the infinite scroll, and use drapload-* attributes to define its options.

使用v-scroll进行滚动翻页,使用 drapload- * 属性来定义它的选项。

```HTML

<div class="app" v-drapload drapload-key="ascroll" drapload-initialize="true" drapload-down="down_a()" drapload-up="up_a()">
  <div>
    <div class="item" v-for="item in a">
      <h1 class="name">{{item.value}}</h1>
      <div class="desc">{{item.data.description}}</div>
      <div class="down">{{item.data.url}}</div>
      <div class="score">{{item.data.suggested_score}}</div>
    </div>
  </div>
</div>
```

```JavaScript
  var app = new Vue({
    el: 'body',
    data: function () {
      return { a: [], b: [] }
    },
    ready: function () {
      var me = this;
      me.$options.vue = me
    },
    /**
     * 加载数据
     * @param fn
     */
    loadListData: function (fn) {
      var me = this.vue;
      $.ajax({
        url: 'npm',
        data: {},
        type: 'GET',
        success: function (data) {
          fn(data.sections.packages)
        }
      });
    },
    methods: {
      down_a: function () {
        var me = this
        me.$options.loadListData(function (data) {
            me.a = me.a.concat(data)
            // 通过设置的key 方法下拉对象方法
            // 如果没有更多数据。你可以 调用 me.ascroll.noData()
            me.ascroll.resetload(true)
        });

      },
      up_a: function () {
        var me = this
        me.$options.loadListData(function (data) {
              me.a = data
              me.ascroll.resetload()
        });
      }
    }
  })
```




# 配置//Config

```JavaScript
  //现在使用默认配置。下拉刷新的各种状态会提供配置选项控制。参考 config.html 页面例子
  var config = {
      domUp: {
          initialCall: function () {}, //初始化状态
          pullingCall: function () {}, //下拉过程中
          loadingCall: function () {},//加载中
          loadEndCall: function () {}//加载完成
        },
        domDown: {
          initialCall: function () {},//初始化
          loadingCall: function () {},
          domNoData: function () {}
        }
    }
```

# 选项//Options

| scroll/Option | Description |
| ----- | ----- |
| drapload-key | 标准变量：该scroll唯一标示．// Number(default = 'scroll')： 你可以在vm对象中找到他 |
| drapload-up | 下拉刷新的时候回调该方法，存在该属性才会支持下拉刷新。否则不支持下拉刷新 |
| drapload-down |加载更多的时候调用该方法，存在该方法才会支持加载更多|
| drapload-initialize | 布尔(默认为false)：设定为true时将在页面加载完成后触发一次drapload-down 对应的方法．|

# 方法//Function

| scroll/Option | Description |
| ----- | ----- |
| resetload | 重置下拉刷新或者加载跟多状态。一般当你加载到数据后会调用该方法 |
| noData | 当加载更多数据（drapload-down对应方法）的时候没有数据后可以调用该方法。这时加载跟多组件状态会改为无数据状态 |
| hasData | 当加载更多数据（drapload-down对应方法）的时候还存在数据后可以调用该方法。这时加载跟多组件状态会改为还能加载更多数据的状态|

# License

MIT