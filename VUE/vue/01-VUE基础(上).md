### 创建应用实例

#### 创建vue应用实例

* 每个 Vue 应用都是通过 [`createApp`](https://cn.vuejs.org/api/application.html#createapp) 函数创建一个新的 **应用实例**：

  创建vue应用	(对象组件/根组件)

  ```
  import { createApp } from 'vue'
  
  const app = createApp({
    /* 根组件选项 */
  })
  ```

#### 挂载应用

* 应用实例必须在调用了 `.mount()` 方法后才会渲染出来。该方法接收一个“容器”参数，可以是一个实际的 DOM 元素或是一个 CSS 选择器字符串：

  ```
  <div id="app"></div>
  ```

  ```
  app.mount('#app')
  ```

  应用根组件的内容将会被渲染在容器元素里面。容器元素自己将**不会**被视为应用的一部分。

  `.mount()` 方法应该始终在整个应用配置和资源注册完成后被调用。同时请注意，不同于其他资源注册方法，它的返回值是根组件实例而非应用实例。

#### 应用配置

* 应用实例会暴露一个 `.config` 对象允许我们配置一些应用级的选项，例如定义一个应用级的错误处理器，它将捕获所有由子组件上抛而未被处理的错误：

  ```
  app.config.errorHandler = (err) => {
    /* 处理错误 */
  }
  ```

  * 应用实例还提供了一些方法来注册应用范围内可用的资源，例如：

  *注册一个组件*

  ```
  app.component('TodoDeleteButton', TodoDeleteButton)
  ```

  * 这使得 `TodoDeleteButton` 在应用的任何地方都是可用的。我们会在指南的后续章节中讨论关于组件和其他资源的注册。你也可以在 [API 参考](https://cn.vuejs.org/api/application.html)中浏览应用实例 API 的完整列表。

  * 确保在挂载应用实例之前完成所有应用配置

***

### 模板语法

#### 文本插值

```html
<span>Message: {{ msg }}</span>
```

#### 原始html

* 这里用到v-html将文本转换

```html
<p>Using text interpolation: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

#### Attribute 绑定

* 想要响应式地绑定一个 attribute，应该使用 [`v-bind` 指令](https://cn.vuejs.org/api/built-in-directives.html#v-bind)：

```html
<div v-bind:id="dynamicId"></div>
```

`v-bind` 指令指示 Vue 将元素的 `id` attribute 与组件的 `dynamicId` 属性保持一致。如果绑定的值是 `null` 或者 `undefined`，那么该 attribute 将会从渲染的元素上移除。

* 简写：

```html
<div :id="dynamicId"></div>
```

* 布尔型 attribute

(布尔值属性) 依据 true / false 值来决定 attribute 是否应该存在于该元素上。[`disabled`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/disabled) 就是最常见的例子之一。`v-bind` 在这种场景下的行为略有不同：

```
<button :disabled="isButtonDisabled">Button</button>
```

当 `isButtonDisabled` 为[真值](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)或一个空字符串 (即 `<button disabled="">`) 时，元素会包含这个 `disabled` attribute。而当其为其他[假值](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)时 attribute 将被忽略。

* 动态绑定多值

```
const objectOfAttrs = {
  id: 'container',
  class: 'wrapper'
}
```

通过不带参数的 `v-bind`，你可以将它们绑定到单个元素上：

```
<div v-bind="objectOfAttrs"></div>
```

#### 使用 JavaScript 表达式(仅支持表达式)

*  Vue 实际上在所有的数据绑定中都支持完整的 JavaScript 表达式：

  ```html
  {{ number + 1 }}
  
  {{ ok ? 'YES' : 'NO' }}
  
  {{ message.split('').reverse().join('') }}
  
  <div :id="`list-${id}`"></div>
  ```

* 这些表达式都会被作为 JavaScript ，以组件为作用域解析执行。

  在 Vue 模板内，JavaScript 表达式可以被使用在如下场景上：

  - 在文本插值中 (双大括号)
  - 在任何 Vue 指令 (以 `v-` 开头的特殊 attribute) attribute 的值中

#### 调用函数

可以在绑定的表达式中使用一个组件暴露的方法：

```html
<span :title="toTitleDate(date)">
  {{ formatDate(date) }}
</span>
```

#### 受限的全局访问

* 模板中的表达式将被沙盒化，仅能够访问到[有限的全局对象列表](https://github.com/vuejs/core/blob/main/packages/shared/src/globalsWhitelist.ts#L3)。该列表中会暴露常用的内置全局对象，比如 `Math` 和 `Date`。

  没有显式包含在列表中的全局对象将不能在模板内表达式中访问，例如用户附加在 `window` 上的属性。然而，你也可以自行在 [`app.config.globalProperties`](https://cn.vuejs.org/api/application.html#app-config-globalproperties) 上显式地添加它们，供所有的 Vue 表达式使用。

### 指令 Directives

* v-if：指令 attribute 的期望值为一个 JavaScript 表达式 ，指令的任务是在其表达式的值变化时响应式地更新 DOM

```html
<p v-if="seen">Now you see me</p>
```

这里，`v-if` 指令会基于表达式 `seen` 的值的真假来移除/插入该 `<p>` 元素。

#### 参数 Arguments

* 某些指令会需要一个“参数”，在指令名后通过一个冒号隔开做标识。例如用 `v-bind` 指令来响应式地更新一个 HTML attribute：

```html
<a v-bind:href="url"> ... </a>

<!-- 简写 -->
<a :href="url"> ... </a>
```

这里 `href` 就是一个参数，它告诉 `v-bind` 指令将表达式 `url` 的值绑定到元素的 `href` attribute 上。在简写中，参数前的一切 (例如 `v-bind:`) 都会被缩略为一个 `:` 字符。

* 另一个例子是 `v-on` 指令，它将监听 DOM 事件：

```html
<a v-on:click="doSomething"> ... </a>

<!-- 简写 -->
<a @click="doSomething"> ... </a>
```

这里的参数是要监听的事件名称：`click`。`v-on` 有一个相应的缩写，即 `@` 字符。我们之后也会讨论关于事件处理的更多细节。

#### 动态参数

* 同样在指令参数上也可以使用一个 JavaScript 表达式，需要包含在一对方括号内：

  ```html
  <!--
  注意，参数表达式有一些约束，
  参见下面“动态参数值的限制”与“动态参数语法的限制”章节的解释
  -->
  <a v-bind:[attributeName]="url"> ... </a>
  
  <!-- 简写 -->
  <a :[attributeName]="url"> ... </a>
  ```

* 相似地，你还可以将一个函数绑定到动态的事件名称上：

  ```html
  <a v-on:[eventName]="doSomething"> ... </a>
  
  <!-- 简写 -->
  <a @[eventName]="doSomething">
  ```

  在此示例中，当 `eventName` 的值是 `"focus"` 时，`v-on:[eventName]` 就等价于 `v-on:focus`。

#### 动态参数值的限制

* 动态参数中表达式的值应当是一个字符串，或者是 `null`。特殊值 `null` 意为显式移除该绑定。其他非字符串的值会触发警告。

#### 动态参数语法的限制

* 动态参数表达式因为某些字符的缘故有一些语法限制，比如空格和引号，在 HTML attribute 名称中都是不合法的。

* 当使用 DOM 内嵌模板 (直接写在 HTML 文件里的模板) 时，我们需要避免在名称中使用大写字母，因为浏览器会强制将其转换为小写

### 修饰符 

* 修饰符是以点开头的特殊后缀，表明指令需要以一些特殊的方式被绑定。例如 `.prevent` 修饰符会告知 `v-on` 指令对触发的事件调用 `event.preventDefault()`：

```html
<form @submit.prevent="onSubmit">...</form>
```

之后在讲到 [`v-on`](https://cn.vuejs.org/guide/essentials/event-handling.html#event-modifiers) 和 [`v-model`](https://cn.vuejs.org/guide/essentials/forms.html#modifiers) 的功能时，你将会看到其他修饰符的例子。

![s](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220921165702509.png)

### 响应式基础

#### 声明响应式状态

我们可以使用 [`reactive()`](https://cn.vuejs.org/api/reactivity-core.html#reactive) 函数创建一个响应式对象或数组：

```js
import { reactive } from 'vue'

const state = reactive({ count: 0 })
```

响应式对象其实是 [JavaScript Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，其行为表现与一般对象相似。不同之处在于 Vue 能够跟踪对响应式对象属性的访问与更改操作。

* 要在组件模板中使用响应式状态，需要在 `setup()` 函数中定义并返回。

  ```
  //引用为创建响应式对象
  <script>
  import { reactive } from 'vue'
  
  export default {
    // `setup` 是一个专门用于组合式 API 的特殊钩子函数
    setup() {
      const state = reactive({ count: 0 })
  
      // 暴露 state 到模板
      return {
        state
      }
    }
  }
  </script>
  ```

  template

  ```
  <div>{{ state.count }}</div>
  ```

  自然，我们也可以在同一个作用域下定义一个更新 `state` 的函数，并作为一个方法与 `state` 一起暴露出去：

  * 选项式

  ```js
  
  //创建响应式对象
  import { reactive } from 'vue'
  
  export default {
    setup() {
      //创建响应式对象
      const state = reactive({ count: 0 })
  
      function increment() {
        state.count++
      }
  
      // 不要忘记同时暴露 increment 函数
      return {
        state,
        increment
      }
    }
  }
  ```

  暴露的方法通常会被用作事件监听器：

  ```html
  <button @click="increment">
    {{ state.count }}
  </button>
  ```

* 函数中手动暴露大量的状态和方法非常繁琐。幸运的是，我们可以通过使用构建工具来简化该操作。当使用单文件组件（SFC）时，我们可以使用 `<script setup>` 来大幅度地简化代码。

  * 组合式

  ```html
  <script setup>
          //创建响应式对象
  import { reactive } from 'vue'
      //创建响应式对象
  const state = reactive({ count: 0 })
  
  function increment() {
    state.count++
  }
  </script>
  
  <template>
    <button @click="increment">
      {{ state.count }}
    </button>
  </template>
  ```

#### DOM 更新时机

* DOM 的更新并不是同步的。相反，Vue 将缓冲它们直到更新周期的 “下个时机” 以确保无论你进行了多少次声明更改，每个组件都只需要更新一次。

若要等待一个状态改变后的 DOM 更新完成，你可以使用 [nextTick()](https://cn.vuejs.org/api/general.html#nexttick) 这个全局 API：

```js
import { nextTick } from 'vue'

function increment() {
  state.count++
  nextTick(() => {
    // 访问更新后的 DOM
  })
}
```

#### 深层响应性

* 在 Vue 中，状态都是默认深层响应式的。这意味着即使在更改深层次的对象或数组，你的改动也能被检测到。

#### 响应式代理 

`reactive()` 返回的是一个原始对象的 [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，它和原始对象是不相等的：

```js
const raw = {}
const proxy = reactive(raw)

// 代理对象和原始对象不是全等的
console.log(proxy === raw) // false
```

* 只有代理对象是响应式的，更改原始对象不会触发更新。因此，使用 Vue 的响应式系统的最佳实践是 **仅使用你声明对象的代理版本**。

#### `reactive()` 的局限性

* Vue 的响应式系统是通过属性访问进行追踪的，因此我们必须始终保持对该响应式对象的相同引用。当响应式对象引用被替换，初始引用的响应性连接丢失！

```js
let state = reactive({ count: 0 })

// 上面的引用 ({ count: 0 }) 将不再被追踪（响应性连接已丢失！）
state = reactive({ count: 1 })
```

* 同时这也意味着当我们将响应式对象的属性赋值或解构至本地变量时，或是将该属性传入一个函数时，我们会失去响应性：

  ```js
  const state = reactive({ count: 0 })
  
  // n 是一个局部变量，同 state.count
  // 失去响应性连接
  let n = state.count
  // 不影响原始的 state
  n++
  
  // count 也和 state.count 失去了响应性连接
  let { count } = state
  // 不会影响原始的 state
  count++
  
  // 该函数接收一个普通数字，并且
  // 将无法跟踪 state.count 的变化
  callSomeFunction(state.count)
  
  ```

