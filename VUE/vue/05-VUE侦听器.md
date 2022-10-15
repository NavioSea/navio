### 侦听器

* 在组合式 API 中，我们可以使用 [`watch` 函数](https://cn.vuejs.org/api/reactivity-core.html#watch)在每次响应式状态发生变化时触发回调函数：

* 侦听器 watch 和computed（计算属性）一样 是vue组件配置选项中的一个，侦听器本质上也是一个函数 他主要用来[监听](https://so.csdn.net/so/search?q=监听&spm=1001.2101.3001.7020)数据的变化（侦听器 watch主要用来侦听data或computed这二个数据）

* watch 的第一个参数可以是不同形式的“数据源”：它可以是一个 **ref (包括计算属性)、一个响应式对象、一个 getter 函数、或多个数据源组成的数组**：

  ```js
  const x = ref(0)
  const y = ref(0)
  
  // 单个 ref
  watch(x, (newX) => {
    console.log(`x is ${newX}`)
  })
  
  // getter 函数
  watch(
    () => x.value + y.value,
    (sum) => {
      console.log(`sum of x + y is: ${sum}`)
    }
  )
  
  // 多个来源组成的数组
  watch([x, () => y.value], ([newX, newY]) => {
    console.log(`x is ${newX} and y is ${newY}`)
  })
  
  ```

  * 不能直接侦听响应式对象的属性值,必须是响应式对象

  ```js
  const obj = reactive({ count: 0 })
  
  // 错误，因为 watch() 得到的参数是一个 number
  watch(obj.count, (count) => {
    console.log(`count is: ${count}`)
  })
  ```

  * 需要用一个返回该属性的 getter 函数

  ```js
  // 提供一个 getter 函数
  watch(
    () => obj.count,
    (count) => {
      console.log(`count is: ${count}`)
    }
  )
  
  ```

#### 深层侦听器

* 直接给 `watch()` 传入一个响应式对象，会隐式地创建一个深层侦听器——该回调函数在所有嵌套的变更时都会被触发：

```
const obj = reactive({ count: 0 })

watch(obj, (newValue, oldValue) => {
  // 在嵌套的属性变更时触发
  // 注意：`newValue` 此处和 `oldValue` 是相等的
  // 因为它们是同一个对象！
})

obj.count++
```

* 相比之下，一个返回响应式对象的 getter 函数，只有在返回不同的对象时，才会触发回调：

```js
watch(
  () => state.someObject,
  () => {
    // 仅当 state.someObject 被替换时触发
  }
)
```

* 加上 `deep` 选项，强制转成深层侦听器

```js
watch(
  () => state.someObject,
  (newValue, oldValue) => {
    // 注意：`newValue` 此处和 `oldValue` 是相等的
    // *除非* state.someObject 被整个替换了
  },
  { deep: true }
)
```

#### `watchEffect()` //立即监听，在创建侦听器时，立即执行一遍回调（`watch()` 是懒执行的）

```js
const url = ref('https://...')
const data = ref(null)

async function fetchData() {
  const response = await fetch(url.value)
  data.value = await response.json()
}

// 立即获取
fetchData()
// ...再侦听 url 变化
watch(url, fetchData)
```

* 用 [`watchEffect` 函数](https://cn.vuejs.org/api/reactivity-core.html#watcheffect) 来简化上面的代码。`watchEffect()` 会立即执行一遍回调函数，如果这时函数产生了副作用，`Vue `会自动追踪副作用的依赖关系，自动分析出响应源。

上面的例子可以重写为：

```js
watchEffect(async () => {
  const response = await fetch(url.value)
  data.value = await response.json()
})
```

* 自动执行回调函数,执行期间，函数产生了副作用，它会自动追踪 （副作用）`url.value` 作为依赖（和计算属性的行为类似）。每当 `url.value` 变化时，**回调会再次执行**

* watch是监听数据源的变化，仅在数据源改变触发回调，容易控制触发时机

#### 回调的触发时机

* **更改响应式状态，它可能会同时触发 `Vue `组件更新和侦听器回调。**

* **创建的侦听器回调，都会在 `Vue`组件更新**之前**被调用**

* **如果想在侦听器回调中能访问被 Vue 更新**之后**的DOM，你需要指明 `flush: 'post'` 选项**

```js
//可以访问到被vue组件更新之后的dom
watch(source, callback, {
  flush: 'post'
})

watchEffect(callback, {
  flush: 'post'
})
```

* 后置刷新的 `watchEffect()` 有个更方便的别名 `watchPostEffect()`：

```js
import { watchPostEffect } from 'vue'

watchPostEffect(() => {
  /* 在 Vue 更新后执行 */
})
```

#### 停止侦听器

* 用同步语句创建的侦听器，会自动绑定到宿主组件实例上，并且会在宿主组件卸载时自动停止

* 用异步回调创建一个侦听器，那么它不会绑定到当前组件上，你必须手动停止它，以防内存泄漏

```
<script setup>
import { watchEffect } from 'vue'

// 它会自动停止
watchEffect(() => {})

// ...这个则不会！
setTimeout(() => {
  watchEffect(() => {})
}, 100)
</script>
------------------------------停止侦听器------------------------------------------
const unwatch = watchEffect(() => {})

// ...当该侦听器不再需要时
unwatch()
```































