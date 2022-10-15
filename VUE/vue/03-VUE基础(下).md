### 事件处理

#### 监听事件

事件处理器的值可以是：

用法：`v-on:click="methodName"` 或 `@click="handler"`。

1. **内联事件处理器**：事件被触发时执行的内联 JavaScript 语句 (与 `onclick` 类似)。
2. **方法事件处理器**：一个指向组件上定义的方法的属性名或是路径。

#### 内联事件处理器

内联事件处理器通常用于简单场景，例如：

```js
const count = ref(0)
```

```html
<template>
    <button @click="count++">Add 1</button>
	<p>Count is: {{ count }}</p>
  	<button @click="count++">Add 1</button>
	<p>Count is: {{ count }}</p>
</template>
```

#### 方法事件处理器

* 方法调用

```js
const name = ref('Vue.js')

function greet(event) {
  alert(`Hello ${name.value}!`)
  // `event` 是 DOM 原生事件
  if (event) {
    alert(event.target.tagName)
  }
}
```

```html
<!-- `greet` 是上面定义过的方法名 -->
<button @click="greet">Greet</button>
```

#### 在内联事件处理器中访问事件参数

* 变量和箭头函数

```html
<!-- 使用特殊的 $event 变量 -->
<button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>

<!-- 使用内联箭头函数 -->
<button @click="(event) => warn('Form cannot be submitted yet.', event)">
  Submit
</button>
```

```js
function warn(message, event) {
  // 这里可以访问原生事件
  if (event) {
    event.preventDefault()
  }
  alert(message)
}
```

### 事件修饰符

* 在处理事件时调用 `event.preventDefault()` 或 `event.stopPropagation()` 是很常见的。尽管我们可以直接在方法内调用，但如果方法能更专注于数据逻辑而不用去处理 DOM 事件的细节会更好

**Vue 为 `v-on` 提供了**事件修饰符**，处理DOM事件细节。修饰符是用 `.` 表示的指令后缀，包含以下这**些：

- `.stop`    //事件停止传递
- `.prevent`  //不再重新加载页面
- `.self`
- `.capture`
- `.once`
- `.passive`

```
<!-- 单击事件将停止传递 -->
<a @click.stop="doThis"></a>
```

```
<!-- 提交事件将不再重新加载页面 -->
<form @submit.prevent="onSubmit"></form>
```

```
<!-- 修饰语可以使用链式书写 -->
<a @click.stop.prevent="doThat"></a>
```

```
<!-- 也可以只有修饰符 -->
<form @submit.prevent></form>
```

```html
<!-- 仅当 event.target 是元素本身时才会触发事件处理器 -->
<!-- 例如：事件处理器不来自子元素 -->
<div @click.self="doThat">...</div>
```

* **`.capture`、`.once` 和 `.passive` 修饰符与[原生 `addEventListener` 事件](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener#options)相对应：**

```html
<!-- 添加事件监听器时，使用 `capture` 捕获模式 -->
<!-- 例如：指向内部元素的事件，在被内部元素处理前，先被外部处理 -->
<div @click.capture="doThis">...</div>

<!-- 点击事件最多被触发一次 -->
<a @click.once="doThis"></a>

<!-- 滚动事件的默认行为 (scrolling) 将立即发生而非等待 `onScroll` 完成 -->
<!-- 以防其中包含 `event.preventDefault()` -->
<div @scroll.passive="onScroll">...</div>

```

###  按键修饰符

* 在监听键盘事件时，我们经常需要检查特定的按键。Vue 允许在 `v-on` 或 `@` 监听按键事件时添加按键修饰符。

```html
<!-- 仅在 `key` 为 `Enter` 时调用 `submit` -->
<input @keyup.enter="submit" />
```

* 你可以直接使用 [`KeyboardEvent.key`](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/key/Key_Values) 暴露的按键名称作为修饰符，但需要转为 kebab-case 形式

```html
<!-- 翻页键 page-down-->
<input @keyup.page-down="onPageDown" />
```

#### 按键别名

- `.enter`
- `.tab`
- `.delete` (捕获“Delete”和“Backspace”两个按键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

#### 系统按键修饰符

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

```html
<!-- Alt + Enter -->
<input @keyup.alt.enter="clear" />

<!-- Ctrl + 点击 -->
<div @click.ctrl="doSomething">Do something</div>
```

#### `.exact` 修饰符

* `.exact` 修饰符允许控制触发一个事件所需的确定组合的系统按键修饰符。

```html
<!-- 当按下 Ctrl 时，即使同时按下 Alt 或 Shift 也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 仅当按下 Ctrl 且未按任何其他键时才会触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 仅当没有按下任何系统按键时触发 -->
<button @click.exact="onClick">A</button>
```

#### 鼠标按键修饰符

- `.left`
- `.right`
- `.middle`

***

### 表单输入绑定

* 手动连接值绑定和事件监听绑定

  ```html
  <input
    :value="text"
    @input="event => text = event.target.value">
  ```

* 上部分等同于v-model(双向绑定)

  ```js
  //v-model 指令帮我们简化了这一步骤：
  <input v-model="text">
  ```

* `v-model` 还可以用于各种不同类型的输入，`<textarea>`、`<select>` 元素。它会根据所使用的元素自动使用对应的 DOM 属性和事件组合：

  * 文本类型 `<input>` 和 `<textarea>` 元素会绑定 `value` property 并侦听 `input` 事件
  * `<input type="checkbox">` 和 `<input type="radio">` 会绑定 `checked` property 并侦听 `change` 事件；
  * `<select>` 会绑定 `value` property 并侦听 `change` 事件：(应该在 JavaScript 中使用响应式系统的 API 来声明该初始值。)

#### 基本的双向绑定

```html
<script setup>
import { ref } from 'vue'

const message = ref('')
</script>

<template>
  <p>Message is: {{ message }}</p>
	<input v-model="message" placeholder="edit me" />
</template>
```

#### 复选框

* 单选

```html
<script setup>
import { ref } from 'vue'

const checked = ref(true)
</script>

<template>
	<input type="checkbox" id="checkbox" v-model="checked" />
	<label for="checkbox">{{ checked }}</label>
</template>
```

* 多选

#### 单选按钮

```html
<script setup>
import { ref } from 'vue'

const picked = ref('One')
</script>

<template>
  <div>Picked: {{ picked }}</div>

	<input type="radio" id="one" value="One" v-model="picked" />
	<label for="one">One</label>

	<input type="radio" id="two" value="Two" v-model="picked" />
  <label for="two">Two</label>
</template>
```

#### 下拉框

* 单选

```html
<script setup>
import { ref } from 'vue'

const selected = ref('')
</script>

<template>
  <span> Selected: {{ selected }}</span>

  <select v-model="selected">
    <option disabled value="">Please select one</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
</template>
```

* 多选

```html
<script setup>
import { ref } from 'vue'

const selected = ref([])
</script>

<template>
  <div>Selected: {{ selected }}</div>

  <select v-model="selected" multiple>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
</template>

<style>
select[multiple] {
  width: 100px;
}
</style>
```

#### `v-for` 动态渲染

```html
<script setup>
import { ref } from 'vue'

const selected = ref('A')

const options = ref([
  { text: 'One', value: 'A' },
  { text: 'Two', value: 'B' },
  { text: 'Three', value: 'C' }
])
</script>

<template>
  <select v-model="selected">
    <option v-for="option in options" :value="option.value">
      {{ option.text }}
    </option>
  </select>

	<div>Selected: {{ selected }}</div>
</template>
```

##### 复选框

* `true-value` 和 `false-value` 是 Vue 特有的 attributes，仅支持和 `v-model` 配套使用。这里 `toggle` 属性的值会在选中时被设为 `'yes'`，取消选择时设为 `'no'`。你同样可以通过 `v-bind` 将其绑定为其他动态值：

```html
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no" />
```

* `v-model` 同样也支持非字符串类型的值绑定！在上面这个例子中，当某个选项被选中，`selected` 会被设为该对象字面量值 `{ number: 123 }`。

```HTML
<select v-model="selected">
  <!-- 内联对象字面量 -->
  <option :value="{ number: 123 }">123</option>
</select>

```

#### 修饰符

##### `.lazy`

* 默认情况下，`v-model` 会在每次 `input` 事件后更新数据

* 可以添加 `lazy` 修饰符来改为在每次 `change` 事件后更新数据

* ```
  <!-- 在 "change" 事件后同步更新而不是 "input" -->
  <input v-model.lazy="msg" />
  ```

##### `.number`

* 输入自动转换为数字

  ```
  <input v-model.number="age" />
  ```

* 如果该值无法被 `parseFloat()` 处理，那么将返回原始值。

  `number` 修饰符会在输入框有 `type="number"` 时自动启用。

##### `.trim`

* 默认自动去除用户输入内容中两端的空格，你可以在 `v-model` 后添加 `.trim` 修饰符：

```
<input v-model.trim="msg" />
```

































