### 生命周期（初始化步骤）

#### 注册周期钩子

* `onMounted` 钩子可以用来在组件完成初始渲染并创建 DOM 节点后运行代码

```html
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  console.log(`the component is now mounted.`)
})
</script>
```

* 还有其他一些钩子，会在实例生命周期的不同阶段被调用，最常用的是 [`onMounted`](https://cn.vuejs.org/api/composition-api-lifecycle.html#onmounted)、[`onUpdated`](https://cn.vuejs.org/api/composition-api-lifecycle.html#onupdated) 和 [`onUnmounted`](https://cn.vuejs.org/api/composition-api-lifecycle.html#onunmounted)

#### 生命周期图

![image-20220922173838190](E:\navio\navio-ft-lz\vue\img\image-20220922173838190.png)

**vue.js生命周期的八大状态：**

**1、beforeCreate（创建前）：vue实例初始化之前调用**

此阶段为实例初始化之后，此时的数据观察和事件配置都还没有准备好，而此时的实例中的data和el还是underfined状态，不可用的，dom元素也未加载,此时使用html片段代码我们加上ref属性，用于获取DOM元素的操作会报错，详细效果请使用代码测试。

**2、created（创建后）：vue实例初始化之后调用**

beforeCreate之后紧接着的钩子就是创建完毕created，此时我们能读取到data的值，但是DOM还没有生成，所以属性el还是不存在的，dom元素也未加载。

**3、beforeMount（载入前）：挂载到DOM树之前调用**

此时的$el成功关联到我们指定的DOM节点，但是此时的DOM元素还未加载，如果此时在DOM元素中绑定数据使用{{name}}后里边的name不能成功地渲染出我们data中的数据

**4、mounted（载入后）：挂载到DOM树之后调用**

挂载完毕阶段，到了这个阶段数据就会被成功渲染出来。DOM元素也加载出来了，html片段代码我们加上ref属性，可以获取DOM元素。

**5、beforeUpdate（更新前）：数据更新之前调用**

当修改Vue实例的data时，Vue就会自动帮我们更新渲染视图，在这个过程中，Vue提供了beforeUpdate的钩子给我们，在检测到我们要修改数据的时候，更新渲染视图之前就会触发钩子beforeUpdate。html片段代码我们加上ref属性，用于获取DOM元素。Dom元素上的数据还没改变。

**6、updated（更新后）：数据更新之后调用**

此阶段为更新渲染视图之后，此时再读取视图上的内容，已经是最新的内容。 此时加载的DOM元素上的数据更新了。

**7、beforeDestroy（销毁前）：vue实例销毁之前调用**

调用实例的destroy()方法可以销毁当前的组件，在销毁之前，会触发beforeDestroy钩子。

**8、destroyed（销毁后）：vue实例销毁之后调用**

成功销毁之后，会触发destroyed钩子，此时该实例与其他实例的关联已经被清除，它与视图之间也被解绑，此时再修改name的值，试图不在更新，说明实例成功被销毁了。



























