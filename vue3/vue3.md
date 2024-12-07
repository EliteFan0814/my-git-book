## 响应性 api： reactive ref toRef toRefs

### reactive 方法传入一个对象，根据传入的对象，返回一个**深度响应式对象**

```javascript
<script setup lang="ts">
import { ref, toRef, reactive } from 'vue';
const reactiveObj = reactive({
  name: 'fpc',
  age: 18,
  info: { address: '中国' },
});
let torefName = toRef(reactiveObj, 'name');
const handleChange = () => {
  reactiveObj.name = 'syq'
  reactiveObj.info.address = '河南'
};
</script>
<template>
  <div>姓名：{{ reactiveObj.name }}</div>
  <div>地址：{{ reactiveObj.info.address }}</div>
  <button @click="handleChange">切换</button>
</template>
```

### ref 给值类型(String，Number，Boolean，Symbol)添加响应式

```javascript
<script setup lang="ts">
import { ref,onMounted } from 'vue'
// 若模板文件里某个元素或子组件定义了 ref="fpcref"，则这里定义的 fpcref 指的就是这个元素的dom对象或组件的实例对象 ，不管ref里是否传值
const fpcref = ref(); // 或者 const fpcref = ref('这是一个字符串');
const nameRef = ref('范跑跑')
onMounted(() => {
  console.log('fpcref.value', fpcref.value);
});
</script>
<template>
  <div ref="fpcref">{{nameRef}}</div>
</template>
```

### toRef 针对一个响应式对象（reactive 封装）的 prop（属性）创建一个 ref，且保持响应式

```javascript
<script setup lang="ts">
import { ref, toRef, reactive } from 'vue';
const reactiveObj = reactive({
  name: 'fpc',
  age: 18,
  info: { address: '中国' },
});
let torefName = toRef(reactiveObj, 'name');
const handleChange = () => {
  reactiveObj.info.address = '河南';
  torefName.value = 'syq';
};
</script>

<template>
  <div>reactiveObj：{{ reactiveObj.name }}</div>
  <div>reactiveObj：{{ reactiveObj.info.address }}</div>
  <div>torefName：{{ torefName }}</div>
  <button @click="handleChange">切换</button>
</template>
```

### toRefs 将响应式对象（reactive 封装）转成对应的 ref 对象

```javascript
<script setup lang="ts">
import { ref, toRef, reactive, toRefs } from 'vue';
const reactiveObj = reactive({
  name: 'fpc',
  age: 18,
  info: { address: '中国' },
});
let torefsObj = toRefs(reactiveObj);
const handleChange = () => {
  torefsObj.name.value = 'syq';
};
</script>

<template>
  <div>reactiveObj：{{ reactiveObj.name }}</div>
  <div>reactiveObj：{{ reactiveObj.info.address }}</div>
  <div>torefsObj：{{ torefsObj }}</div>
  <button @click="handleChange">切换</button>
</template>
```

### Vue3 中的表单输入绑定

```html
<!-- 原始写法 -->
<input :value="text" @input="event => text = event.target.value" />
<!-- 简化写法 -->
<input v-model="text" />
```

v-model 还可以用于各种不同类型的输入，`<textarea>、<select>` 元素。它会根据所使用的元素自动使用对应的 DOM 属性和事件组合
：

文本类型的 `<input>` 和 `<textarea>` 元素会绑定 value property 并侦听 input 事件；  
`<input type="checkbox">` 和 `<input type="radio">` 会绑定 checked property 并侦听 change 事件；  
`<select>` 会绑定 value property 并侦听 change 事件。

**_对于需要使用 IME 的语言 (中文，日文和韩文等)，你会发现 v-model 不会在 IME 输入还在拼字阶段时触发更新。如果你的确想在
拼字阶段也触发更新，请直接使用自己的 input 事件监听器和 value 绑定而不要使用 v-model。_**

### 计算属性 computed、侦听器 watch、watchEffect

计算属性 computed：  
它的 getter 应只做计算而没有任何其他的副作用！  
不要改变其他状态、不要在 getter 中做异步请求或者更改 DOM！  
一个计算属性的声明中描述的是如何根据其他值派生一个值。因此 getter 的职责应该仅为计算和返回该值。

侦听器 watch：  
需要在状态变化时执行一些“副作用”：例如更改 DOM，或是根据异步操作的结果去修改另一处的状态。

watchEffect：  
对于有多个依赖项的侦听器来说，使用 watchEffect() 可以消除手动维护依赖列表的负担；  
如果你需要侦听一个嵌套数据结构中的几个属性，watchEffect() 可能会比深度侦听器更有效，因为它将只跟踪回调中被使用到的属性，
而不是递归地跟踪所有的属性。

```js
const todoId = ref(1);
const data = ref(null);

// watch写法
watch(
  todoId,
  async () => {
    const response = await fetch(`https://jsonplaceholder.typicode.com/todos/${todoId.value}`);
    data.value = await response.json();
  },
  { immediate: true }
);

// watchEffect 写法
watchEffect(async () => {
  const response = await fetch(`https://jsonplaceholder.typicode.com/todos/${todoId.value}`);
  data.value = await response.json();
});
```
