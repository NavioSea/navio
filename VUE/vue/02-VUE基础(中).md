### 响应式基础

#### 用 `ref()` 定义响应式变量

* Vue 提供了一个 [`ref()`](https://cn.vuejs.org/api/reactivity-core.html#ref) 方法来允许我们创建可以使用任何值类型的响应式 **ref**

```js
import { ref } from 'vue'
const count = ref(0)
```

`ref()` 将传入参数的值包装为一个带 `.value` 属性的 ref 对象：

```js
const count = ref(0)
console.log(count) // { value: 0 }
console.log(count.value) // 0
count.value++
console.log(count.value) // 1
```

* 和响应式对象的属性类似，ref 的 `.value` 属性也是响应式的。同时，当值为对象类型时，会用 `reactive()` 自动转换它的 `.value`。
* 一个包含对象类型值的 ref 可以响应式地替换整个对象：

```js
const objectRef = ref({ count: 0 })
// 这是响应式的替换
objectRef.value = { count: 1 }
```

* ref 被传递给函数或是从一般对象上被解构时，不会丢失响应性：

```js
const obj = {
  foo: ref(1),
  bar: ref(2)
}
// 该函数接收一个 ref
// 需要通过 .value 取值
// 但它会保持响应性
callSomeFunction(obj.foo)
// 仍然是响应式的
const { foo, bar } = obj
```

#### ref 在模板中的解包

* 当 ref 在模板中作为顶层属性被访问时，它们会被自动“解包”，所以不需要使用 `.value`。下面是之前的计数器例子，用 `ref()` 代替：

  ***

  <script setup>
  import { ref } from 'vue'

  const count = ref(0)

  function increment() {
    count.value++
  }
  </script>

  <template>
    <button @click="increment">
      {{ count }} <!-- 无需 .value -->
    </button>
  </template>

  ***

* ```js
  const object = { foo: ref(1) }//object.foo不是顶层属性，无法解包
  ```

* foo “解包”:

  ```js
  const { foo } = object
  ```

  ```js
  {{ foo + 1 }}//foo此时是顶层属性,可被解包
  ```

#### ref 在响应式对象中的解包

* 一个ref被嵌套在响应式对象中，作为属性被修改和访问时会自动解包

* 如果将一个新的 ref 赋值给一个关联了已有 ref 的属性，那么它会替换掉旧的 ref：

* 只有当嵌套在一个深层响应式对象内时，才会发生 ref 解包。当其作为[浅层响应式对象](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive)的属性被访问时不会解包。

#### 数组和集合类型的 ref 解包

* 当 ref 作为响应式数组或像 `Map` 这种原生集合类型的元素被访问时，不会进行解包。

```js
const books = reactive([ref('Vue 3 Guide')])
// 这里需要 .value
console.log(books[0].value)

const map = reactive(new Map([['count', ref(0)]]))
// 这里需要 .value
console.log(map.get('count').value)
```

***

### 计算属性

```js
<script setup>
import { reactive, computed } from 'vue'

const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})

// 一个计算属性 ref
const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})
</script>

<template>
  <p>Has published books:</p>
  <span>{{ publishedBooksMessage }}</span>
</template>

```

* `computed()` 方法期望接收一个 getter 函数，返回值为一个**计算属性 ref**。和其他一般的 ref 类似，你可以通过 `publishedBooksMessage.value` 访问计算结果。计算属性 ref 也会在模板中自动解包，因此在模板表达式中引用时无需添加 `.value`。

* computed可以指定泛型

  ```js
   const double = computed<number>(() => {
    // 若返回值不是 number 类型则会报错
  })
  ```

***

#### 为事件处理函数标注类型

```js
<script setup lang="ts">
function handleChange(event) {
  // `event` 隐式地标注为 `any` 类型
  console.log(event.target.value)
}
</script>

<template>
  <input type="text" @change="handleChange" />
</template>
```

* 没有类型标注时，这个 `event` 参数会隐式地标注为 `any` 类型。可能需要显式地强制转换 `event` 上的属性:

```js
function handleChange(event: Event) {
  console.log((event.target as HTMLInputElement).value)
}
```

#### 为 provide / inject 标注类型

```js
import { provide, inject } from 'vue'
import type { InjectionKey } from 'vue'
//指定要注入Key的泛型类型
const key = Symbol() as InjectionKey<string>

provide(key, 'foo') // 若提供的是非字符串值会导致错误

const foo = inject(key) // foo 的类型：string | undefined
```

* 建议将注入 key 的类型放在一个单独的文件中，这样它就可以被多个组件导入。

* 当使用字符串注入 key 时，注入值的类型是 `unknown`，需要通过泛型参数显式声明：

```js
//指定value注入的泛型类型
const foo = inject<string>('foo') // 类型：string | undefined
#注入的值仍然可以是 undefined,当提供了一个默认值后，这个 `undefined` 类型就可以被移除：
const foo = inject<string>('foo', 'bar') // 类型：string
#如果你确定该值将始终被提供，则还可以强制转换该值：
const foo = inject('foo') as string
```

#### 为模板引用标注类型

* 模板引用需要通过一个显式指定的泛型参数和一个初始值 `null` 来创建：

```js
<script setup lang="ts">
import { ref, onMounted } from 'vue'

const el = ref<HTMLInputElement | null>(null)

onMounted(() => {
  el.value?.focus()
})
</script>

<template>
  <input ref="el" />
</template>

```

***

 #### 计算属性缓存 vs 方法

```
//方法 不会被缓存
<p>{{ calculateBooksMessage() }}</p>
// 组件中 计算属性
function calculateBooksMessage() {
  return author.books.length > 0 ? 'Yes' : 'No'
}

```

* 若我们将同样的函数定义为一个方法而不是计算属性，两种方式在结果上确实是完全相同的，然而，不同之处在于**计算属性值会基于其响应式依赖被缓存**。一个计算属性仅会在其响应式依赖更新时才重新计算。这意味着只要 `author.books` 不改变，无论多少次访问 `publishedBooksMessage` 都会立即返回先前的计算结果，而不用重复执行 getter 函数。

#### 可写计算属性

* 计算属性内自定义get和set方法

  ```js
  <script setup>
  import { ref, computed } from 'vue'
  
  const firstName = ref('John')
  const lastName = ref('Doe')
  
  const fullName = computed({
    // getter
    get() {
      return firstName.value + ' ' + lastName.value
    },
    // setter
    set(newValue) {
      // 注意：我们这里使用的是解构赋值语法
      [firstName.value, lastName.value] = newValue.split(' ')
    }
  })
  </script>
  
  ```

  * **不要在计算函数中做异步请求或者更改 DOM**
  * **从计算属性返回的值是派生状态。可以把它看作是一个“临时快照”，每当源状态发生变化时，就会创建一个新的快照**

***

### Class 与 Style 绑定

#### 绑定对象

* 可以给 `:class` (`v-bind:class` 的缩写) 传递一个对象来动态切换 class

```js
<div :class="{ active: isActive }"></div>
#上面的语法表示 active 是否存在取决于数据属性 isActive 的真假值。
```

* 和标签属性 class 共存

```html
<script>
const isActive = ref(true)
const hasError = ref(false)
</script>
<div
  class="static"ht,l
  :class="{ active: isActive, 'text-danger': hasError }"
></div>
```

渲染结果:

```html
<div class="static active"></div>
```

* 也可以直接绑定一个对象

```html
<script>
const classObject = reactive({
  active: true,
  'text-danger': false
})html
</script>

<div :class="classObject"></div>
```

* 绑定返回对象的计算属性

```js
<script>
const isActive = ref(true)
const error = ref(null)

const classObject = computed(() => ({
  active: isActive.value && !error.value,
  'text-danger': error.value && error.value.type === 'fatal'
}))
</script>
//渲染结果
<div :class="classObject"></div>

```

#### 绑定数组

* 绑定数组

```js
const activeClass = ref('active')
const errorClass = ref('text-danger')
```

```html
<div :class="[activeClass, errorClass]"></div>
```

* 使用变量表达式

```html
<div :class="[isActive ? activeClass : '', errorClass]"></div>
```

* 在数组中嵌套对象：

```html
<div :class="[{ active: isActive }, errorClass]"></div>
```

#### 绑定内联样式

* 内联样式绑定对象、数组、数组对象等

  ```html
   <script>
  const activeColor = ref('red')
  const fontSize = ref(30)
   </script>
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
  ```

***

### 条件渲染

#### v-if和v-else

* `v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回真值时才被渲染

```html
<script setup>
import { ref } from 'vue'
//创建响应式值
const awesome = ref(true)
</script>

<template>
<button @click="awesome = !awesome">Toggle</button>
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
</template>template>
```

* 一个 `v-else` 元素必须跟在一个 `v-if` 或者 `v-else-if` 元素后面，否则它将不会被识别。

#### `<template>` 上的 `v-if`

* 因为 `v-if` 是一个指令，他必须依附于某个元素。想要切换不止一个元素,可以在一个 `<template>` 元素上使用 `v-if`,这只是一个不可见的包装器元素，最后渲染的结果并不会包含这个 `<template>` 元素

  ```html
  <template v-if="ok">
    <h1>Title</h1>
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
  </template>
  ```

#### `v-show`

* 另一个可以用来按条件显示一个元素的指令是 `v-show`。其用法基本一样：

```html
<h1 v-show="ok">Hello!</h1>
```

* 不同之处在于 `v-show` 会在 DOM 渲染中**保留该元素**；`v-show` 仅切换了该元素上名为 `display` 的 **CSS 属性**。
* `v-show` 不支持在 `<template>` 元素上使用，也不能和 `v-else` 搭配使用。

#### `v-if` 和 `v-for`

* 当 `v-if` 和 `v-for` 同时存在于一个元素上的时候，`v-if` 会首先被执行

***

### 列表渲染

#### `v-for`

```js
const items = ref([{ message: 'Foo' }, { message: 'Bar' }])
```

```html
<li v-for="item in items">
  {{ item.message }}
</li>
```

* 在 `v-for` 块中可以完整地访问父作用域内的属性和变量。`v-for` 也支持使用可选的第二个参数表示当前项的位置索引

```html
<script>
const parentMessage = ref('Parent')
const items = ref([{ message: 'Foo' }, { message: 'Bar' }])
</script>
<li v-for="(item, index) in items">
  {{ parentMessage }} - {{ index }} - {{ item.message }}
</li>
```

* 注意 `v-for` 是如何对应 `forEach` 回调的函数签名的。实际上，你也可以在定义 `v-for` 的变量别名时使用解构，和解构函数参数类似：

```html
<li v-for="{ message } in items">
  {{ message }}
</li>

<!-- 有 index 索引时 -->
<li v-for="({ message }, index) in items">
  {{ message }} {{ index }}
</li>
```

* 可以使用 `of` 作为分隔符来替代 `in`，这更接近 JavaScript 的迭代器语法：

```html
<div v-for="item of items"></div>
```

***

#### `v-for` 与对象

* 可以使用 `v-for` 来遍历一个对象的**所有属性**。遍历的顺序会基于对该对象调用 `Object.keys()` 的返回值来决定

* 可以通过提供第二个参数表示属性名 (例如 key)：

```html
<li v-for="(value, key) in myObject">
  {{ key }}: {{ value }}
</li>
```

* 第三个参数表示位置索引：

```html
<li v-for="(value, key, index) in myObject">
  {{ index }}. {{ key }}: {{ value }}
</li>
```

***

#### `v-for` 里使用范围值

* 注意此处 `n` 的初值是从 `1` 开始而非 `0`。

```html
<span v-for="n in 10">{{ n }}</span>
```

* 与模板上的 `v-if` 类似，你也可以在 `<template>` 标签上使用 `v-for` 来渲染一个包含多个元素的块。例如：

```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

* 在外新包装一层 `<template>` 再在其上使用 `v-for` 可以解决这个问题 (这也更加明显易读)：

  ```html
  <template v-for="todo in todos">
    <li v-if="!todo.isComplete">
      {{ todo.name }}
    </li>
  </template>
  ```

#### 数组变化侦测

Vue 能够侦听响应式数组的变更方法，并在它们被调用时触发相关的更新。这些变更方法包括：

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

**替换一个数组**

* 变更方法，顾名思义，就是会对调用它们的原数组进行变更。相对地，也有一些不可变 (immutable) 方法，例如 `filter()`，`concat()` 和 `slice()`，这些都不会更改原数组，而总是**返回一个新数组**。当遇到的是非变更方法时，我们需要将旧的数组替换为新的：

```js
// `items` 是一个数组的 ref
items.value = items.value.filter((item) => item.message.match(/Foo/))
```

#### 展示过滤或排序后的结果(将过滤结果创建返回计算属性)

* **在计算属性可行的情况下**

```js
const numbers = ref([1, 2, 3, 4, 5])

const evenNumbers = computed(() => {
  return numbers.value.filter((n) => n % 2 === 0)
})
```

```html
<li v-for="n in evenNumbers">{{ n }}</li>
```

* **在计算属性不可行的情况下 (例如在多层嵌套的 `v-for` 循环中)**

```js
const sets = ref([
  [1, 2, 3, 4, 5],
  [6, 7, 8, 9, 10]
])

function even(numbers) {
  return numbers.filter((number) => number % 2 === 0)
}
```

```html
<ul v-for="numbers in sets">
  <li v-for="n in even(numbers)">{{ n }}</li>
</ul>
```

* **在计算属性中避免变更原始数组**









