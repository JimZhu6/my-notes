# Vue3 笔记

> [预览版仓库](https://github.com/vuejs/vue-next-webpack-preview)、[Vue3仓库](https://github.com/vuejs/vue-next)



## 新特性

### Composition API

在 Vue3 中，可直接通过引入的方式执行 Vue2 中的各种 Function，代码结构更加精简。

```vue
<template>
  <div class="counter">
    <p>count: {{ count }}</p>
    <p>NewVal (count + 2): {{ countDouble }}</p>
    <button @click="inc">Increment</button>
    <button @click="dec">Decrement</button>
    <p> Message: {{ msg }} </p>
    <button @click="changeMessage()">Change Message</button>
  </div>
</template>
<script>
import { ref, computed, watch } from 'vue'
export default {
  setup() {
    /* ---------------------------------------------------- */
    let count = ref(0)
    const countDouble = computed(() => count.value * 2)
    watch(count, newVal => {
      console.log('count changed', newVal)
    })
    const inc = () => {
      count.value += 1
    }
    const dec = () => {
      if (count.value !== 0) {
        count.value -= 1
      }
    }
    /* ---------------------------------------------------- */
    let msg= ref('some text')
    watch(msg, newVal => {
      console.log('msg changed', newVal)
    })
    const changeMessage = () => {
      msg.value = "new Message"
    }
    /* ---------------------------------------------------- */
    return {
      count,
      inc,
      dec,
      countDouble,
      msg,
      changeMessage
    }
  }
}
</script>
```

我们同样可以抽取出重复功能的代码:

```js
// message.js
import { ref, watch } from "vue";
export function message() {
  let msg = ref(123);
  watch(msg, (newVal) => {
    console.log("msg changed", newVal);
  });
  const changeMessage = () => {
    msg.value = "new Message";
  };
  return { msg, changeMessage };
}
```

在其他组件中使用上面组件:

```vue
<template>
  <div class="counter">
    <p>count: {{ count }}</p>
    <p>NewVal (count + 2): {{ countDouble }}</p>
    <button @click="inc">Increment</button>
    <button @click="dec">Decrement</button>
    <p>Message: {{ msg }}</p>
    <button @click="changeMessage()">change message</button>
  </div>
</template>
<script>
import { ref, computed, watch } from 'vue'
import { message } from './common/message'
export default {
  setup() {
    let count = ref(0)
    const countDouble = computed(() => count.value * 2)
    watch(count, newVal => {
      console.log('count changed', newVal)
    })
    const inc = () => {
      count.value += 1
    }
    const dec = () => {
      if (count.value !== 0) {
        count.value -= 1
      }
    }
    let { msg, changeMessage } = message()
    return {
      count,
      msg,
      changeMessage,
      inc,
      dec,
      countDouble
    }
  }
}
</script>
```



### Multiple root elements

