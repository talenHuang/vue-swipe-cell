# vue-swipe-cell

> it is a vue component about swiper cell
> <br>
> 这是一个滑动删除的 vue 列表组件，比较简单快捷，欢迎大家提 issue，如果觉得对您有帮助，请不要吝啬您的小星星！

## Build Setup

```bash
# 安装依赖
npm install vue-swipe-cell

#引入依赖
import swipeCell from 'vue-swipe-cell';

#使用依赖
Vue.use(swipeCell);

#直接在页面引用组件
<swipe-cell></swipe-cell>
```

## How to Use

| 属性     | 作用                    | 例子                                              |
| -------- | ----------------------- | ------------------------------------------------- |
| title    | cell 左侧的值           | \<swipe-cell :title="title">\</swipe-cell>        |
| del 事件 | cell 默认事件为删除事件 | \<swipe-cell @del="你的自定义事件">\</swipe-cell> |
| slot     | 操作按钮自定义插槽      | 见下面 demo                                       |

默认监听事件:`del`

## Demo

- 无需自己配置，拿来即用，可以参考如下写法

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

- 也可自定义操作组件

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
