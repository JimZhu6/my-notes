## vue

### Vue DOM元素绑定动态style

```html
:style="{ transform: 'translateY(-' + num1 + 'px)' }"
```

### Vue dom元素绑定多个类名需要以数组的方式书写

```html
:class="[item.colorAndBoxName,
         current1==item.id?'selected1':'',
         current2==item.id?'selected2':'',
         current3==item.id?'selected3':'']"
```

### requestAnimationFrame方法写动画

[RAF参考教程](https://javascript.ruanyifeng.com/htmlapi/requestanimationframe.html#toc0)

```javascript
let animation3Func = function() {
    // do something...
    // 需要执行自调用来执行动画效果
    self.animalId3 = window.requestAnimationFrame(animation3Func);
  };
self.animalId3 = window.requestAnimationFrame(animation3Func);
//清除这个动画效果
window.cancelAnimationFrame(this.animalId3);
```

### Vue使用clipboard 1/2（将指定数据存储到剪贴板）

[git地址](https://github.com/zenorocha/clipboard.js)

使用clipboard 2全局注册

```javascript
import VueClipboard from 'vue-clipboard2'
Vue.use(VueClipboard)
```


#### 全局注册时用法

```html
<template>
<div class="container">
    <input type="text" v-model="message">
    <button type="button"
      v-clipboard:copy="message"
      v-clipboard:success="onCopy"
      v-clipboard:error="onError">Copy!</button>
  </div>
</template>

<script>
new Vue({
  el: '#app',
  template: '#t',
  data: function () {
    return {
      message: 'Copy These Text'
    }
  },
  methods: {
    onCopy: function (e) {
      alert('You just copied: ' + e.text)
    },
    onError: function (e) {
      alert('Failed to copy texts')
    }
  }
})
</script>
```



#### 使用clipboard局部组件注册

```html
<span ref="copy"
      data-clipboard-action="copy"
      :data-clipboard-text="arr"
      @click="copy">复制结果</span>
```



```javascript
import clipboard from "clipboard";
export default {
  data(){
    return{
      //动态数组
      arr:[],
      //存储clipboard实例
      copyBtn: null,
      //状态码
      status: 0,
      //待复制内容
      message: ""
    }
  },
    mounted() {
    // 初始化clipboard绑定事件
    //refs里的元素是点击时执行复制操作的按钮
    this.copyBtn = new clipboard(this.$refs.copy);
  },
  methods:{
    copy: function() {
      let _clipboard = this.copyBtn;
      let self = this;
      _clipboard.on("success", function(e) {
        self.status = 1;
        self.message = "你已复制到如下号码:" + e.text;
      });
    }
  },
  watch: {
    status(e) {
      if (e === 1) {
        alert(this.message);
        this.status = 0;
      }
    }
  },
}

```




