# vue-swipe-cell

> 这是一个滑动删除的 vue 列表组件，兼容 Android4.4.4 以上（部分 Android4.4.0）webview，比较简单快捷，欢迎大家提 issue，如果觉得对您有帮助，请不要吝啬您的小星星！
> <br>

> It is a vue component about swiper cell and compatible above of Android4.4.4(part of Android4.4.0) webview. it`s so convenient. it is my pleasure if you submit issues. It is also important to give me a start if that is helpful to you!

## Build Setup

```bash
# 安装依赖
#install  dependenties
npm install vue-swipe-cell

#引入依赖
#import dependenties
import swipeCell from 'vue-swipe-cell';

#使用依赖
#use dependenties
Vue.use(swipeCell);

#直接在页面引用组件
#use component on page directly
<swipe-cell></swipe-cell>
```

## How to Use

_中文_

| 属性                           | 作用                    | 例子                                              |
| ------------------------------ | ----------------------- | ------------------------------------------------- |
| title                          | cell 左侧的值           | \<swipe-cell :title="title">\</swipe-cell>        |
| del 事件                       | cell 默认事件为删除事件 | \<swipe-cell @del="你的自定义事件">\</swipe-cell> |
| slot(默认插槽)                 | 操作按钮自定义插槽      | 见下面 demo                                       |
| slot(name='content')(内容插槽) | 左侧内容自定义          | 见下面 demo                                       |

默认监听事件:`del`

_English_
| property |effect | example |
| ------------------------------ | ----------------------- | ------------------------------------------------- |
| title | cell`s left value | \<swipe-cell :title="title">\</swipe-cell> |
| del event | cell (default event is delete) | \<swipe-cell @del="">\</swipe-cell> |
| slot(default slot) | action button customize | as follow demo |
| slot(name='content')(content slot) | content customize | as follow demo |

default listening event:`del`

## Demo

- 无需自己配置，拿来即用，可以参考如下写法
- can`t configure,you can copy to use directly

```javascript
<template>

  <div id="app">
    <swipe-cell :title="title" @del="del"></swipe-cell>
  </div>

</template>

<script>
export default {
    data() {
    return {
      title: "我是title"
    };
    },
  methods: {
    del() {
      alert("点击了删除按钮");
    }
  }
};
</script>
```

效果如下：
![](https://i.imgur.com/LRSvwAO.gif)

- 也可自定义操作组件（默认插槽）
- you can also configure action button slot customly,as follows:

```javascript
<template>

  <div id="app">
    <swipe-cell :title="title" >
        <button @click="del" style="background:blue">编辑</button>
        <button @click="add" style="background:yellowgreen">增加</button>
    </swipe-cell>
  </div>

</template>

<script>
export default {
    data() {
    return {
      title: "我是title"
    };
    },
  methods: {
    del() {
      alert("点击了删除按钮");
    },
    add() {
      alert("点击了增加按钮");
    }
  }
};
</script>
```

效果如下：
![](https://i.imgur.com/N9qM2w8.gif)

- 也可以自定义内容
- you can also configure content customly,as follows:

```javascript
<template>
  <div id="app">

    <swipe-cell :title="title" v-for="(item,index) in ['大梅','洪立']" :key="index">

      <div class="content" slot="content">
        <img src="./assets/logo.png" alt="">
        <ul>
          <li> {{item}}</li>
          <li> 2333</li>
          <li> 6666</li>
        </ul>
      </div>

      <button @click="del" style="background:blue">编辑</button>
      <button @click="add" style="background:yellowgreen">增加</button>

    </swipe-cell>

  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      title: "我是title"
    };
  },
  methods: {
    del() {
      alert("点击了删除按钮");
    },

    add() {
      alert("点击了增加按钮");
    }
  }
};
</script>
```

效果如图所示：
![](https://i.imgur.com/Zed8yzw.gif);
