### 模板引用

**`ref` 是一个特殊的 attribute，和 `v-for` 章节中提到的 `key` 类似。它允许我们在一个特定的 DOM 元素或子组件实例被挂载后，获得对它的直接引用**

#### 访问模板引用

* 通过组合式 API 获得该模板引用，我们需要声明一个同名的 ref

```html
<script setup>
import { ref, onMounted } from 'vue'

// 声明一个 ref 来存放该元素的引用
    
// 必须和模板里的 ref 同名
const input = ref(null)

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="input" />
</template>

```

* 如果不使用 `<script setup>`，需确保从 `setup()` 返回 ref

```js
export default {
  setup() {
    const input = ref(null)
    // ...
    return {
      input
    }
  }
}
```

* 挂载后侦听 input值改变

```js
watchEffect(() => {
  if (input.value) {
    input.value.focus()
  } else {
    // 此时还未挂载，或此元素已经被卸载（例如通过 v-if 控制）
  }
})
```

#### `v-for` 中的模板引用

* 当在 `v-for` 中使用模板引用时，对应的 ref 中包含的值是一个数组，它将在元素被挂载后包含对应整个列表的所有元素：

```html
<script setup>
import { ref, onMounted } from 'vue'

const list = ref([1,2,3,4])

const itemRefs = ref([])

onMounted(() => {
  alert(itemRefs.value.map(i => i.textContent))
})
</script>

<template>
  <ul>
    <li v-for="item in list" ref="itemRefs">
      {{ item }}
    </li>
  </ul>
</template>
```

#### 函数模板引用

* `ref` attribute 还可以绑定为一个函数，会在每次组件更新时都被调用。该函数会收到元素引用作为其第一个参数：

<input :ref="(el) => { /* 将 el 赋值给一个数据属性或 ref 变量 */ }">

* 需要使用动态的 `:ref` 绑定才能够传入一个函数。当绑定的元素被卸载时，函数也会被调用一次，此时的 `el` 参数会是 `null`,**同理也可绑定方法**

### 组件基础

#### defineCustomElement

* Vue 提供了一个和定义一般 Vue 组件几乎完全一致的 [`defineCustomElement`](https://cn.vuejs.org/api/general.html#definecustomelement) 方法来支持创建自定义元素。这个方法接收的参数和 [`defineComponent`](https://cn.vuejs.org/api/general.html#definecomponent) 完全相同。但它会返回一个继承自 `HTMLElement` 的自定义元素构造器：

  ```html
  <my-vue-element></my-vue-element>
  ```

  ```js
  import { defineCustomElement } from 'vue'
  
  const MyVueElement = defineCustomElement({
    // 这里是同平常一样的 Vue 组件选项
    props: {},
    emits: {},
    template: `...`,
  
    // defineCustomElement 特有的：注入进 shadow root 的 CSS
    styles: [`/* inlined css */`]
  })
  
  // 注册自定义元素
  // 注册之后，所有此页面中的 `<my-vue-element>` 标签
  // 都会被升级
  customElements.define('my-vue-element', MyVueElement)
  
  // 你也可以编程式地实例化元素：
  // （必须在注册之后）
  document.body.appendChild(
    new MyVueElement({
      // 初始化 props（可选）
    })
  )
  ```

#### 使用组件

* 使用一个子组件，我们需要在父组件中导入它。假设我们把计数器组件放在了一个叫做 `ButtonCounter.vue` 的文件中，这个组件将会以默认导出的形式被暴露给外部。

```html
<script setup>
import ButtonCounter from './ButtonCounter.vue'
</script>

<template>
  <h1>Here is a child component!</h1>
  <ButtonCounter />
</template>
```

* 通过 `<script setup>`，导入的组件都在模板中直接可用。

* 组件可以被重用任意多次：

```html
<h1>Here is a child component!</h1>
<ButtonCounter />
<ButtonCounter />
<ButtonCounter />
```

* 每使用一个组件，就相当于创建一个新实例，是互相区分的

#### 传递 props

* Props 是一种特别的 attributes，你可以在组件上声明注册。要传递给博客文章组件一个标题，我们必须在组件的 props 列表上声明它。这里要用到 [`defineProps`](https://cn.vuejs.org/api/sfc-script-setup.html#defineprops-defineemits)

```html
<!-- BlogPost.vue -->
<script setup>
defineProps(['title'])
</script>

<template>
  <h4>{{ title }}</h4>
</template>

```

* 如果没有使用到<script setup>，props 必须以 props 选项的方式声明，props 对象会作为 setup() 函数的第一个参数被传入

```js
export default {
  props: ['title'],
  setup(props) {
    console.log(props.title)
  }
}
```

* 一个组件可以有任意多的 props，默认情况下，所有 prop 都接受任意类型的值。

  当一个 prop 被注册后，可以像这样以自定义 attribute 的形式传递数据给它:

  ```html
  <BlogPost title="My journey with Vue" />
  <BlogPost title="Blogging with Vue" />
  <BlogPost title="Why Vue is so fun" />
  ```

* 在实际应用中，我们可能在父组件中会有如下的一个博客文章数组

```js
const posts = ref([
  { id: 1, title: 'My journey with Vue' },
  { id: 2, title: 'Blogging with Vue' },
  { id: 3, title: 'Why Vue is so fun' }
])
```

这种情况下，我们可以使用 `v-for` 来渲染它们：

```html
<BlogPost
  v-for="post in posts"
  :key="post.id"
  :title="post.title"
 />
```

#### 监听事件

* 子组件可以通过调用内置的 [**`$emit`** 方法](https://cn.vuejs.org/api/component-instance.html#emit)，通过传入事件名称来抛出一个事件：

我们可以通过 [`defineEmits`](https://cn.vuejs.org/api/sfc-script-setup.html#defineprops-defineemits) 宏来声明需要抛出的事件

声明了一个组件可能触发的所有事件，还可以对事件的参数进行[验证](https://cn.vuejs.org/guide/components/events.html#validate-emitted-events)。同时，这还可以让 Vue 避免将它们作为原生事件监听器隐式地应用于子组件的根元素。

* **BlogPost.vue**

```html
<script setup>
//声明属性    
defineProps(['title'])
//我们可以通过 defineEmits 宏来声明需要抛出的事件：    
defineEmits(['enlarge-text'])
</script>

<template>
  <div class="blog-post">
    <h4>{{ title }}</h4>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>
```

* **App.vue**

```html
<script setup>
import { ref } from 'vue'
import BlogPost from './BlogPost.vue'
  
const posts = ref([
  { id: 1, title: 'My journey with Vue' },
  { id: 2, title: 'Blogging with Vue' },
  { id: 3, title: 'Why Vue is so fun' }
])

const postFontSize = ref(1)
</script>

<template>
	<div :style="{ fontSize: postFontSize + 'em' }">
    <BlogPost
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      @enlarge-text="postFontSize += 0.1"
    ></BlogPost>
  </div>
</template>
```

* 和 `defineProps` 类似，`defineEmits` 仅可用于 `<script setup>` 之中，并且不需要导入，它返回一个等同于 `$emit` 方法的 `emit` 函数。它可以被用于在组件的 `<script setup>` 中抛出事件，因为此处无法直接访问 `$emit`

```html
<script setup>
const emit = defineEmits(['enlarge-text'])

emit('enlarge-text')
</script>
```

* 如果你没有在使用 `<script setup>`，你可以通过 `emits` 选项定义组件会抛出的事件。你可以从 `setup()` 函数的第二个参数，即 setup 上下文对象上访问到 `emit` 函数：

  ```js
  export default {
    emits: ['enlarge-text'],
    setup(props, ctx) {
      ctx.emit('enlarge-text')
    }
  }
  ```

#### 动态组件

#### 通过 Vue 的 `<component>` 元素和特殊的 `is` attribute 实现的：

```html
<!-- currentTab 改变时组件也改变 -->
<component :is="tabs[currentTab]"></component>
```

