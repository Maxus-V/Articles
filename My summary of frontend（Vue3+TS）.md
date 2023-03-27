# My summary of Vue3



## Part 1

（Difference between Vue2 and Vue3）

### 1. Changes in monitoring mechanisms

- Vue3 uses ES6 Proxy API to proxy data, monitoring the entire object, rather than a property.
- Removing many of the limitations of Object.define Property based implementations in Vue 2.
- Vue3 can monitor the addition and deletion of object properties, and can monitor the changes of arrays.
- Vue3 supports Map/Set/WeakMap/WeakSet.



### 2. Vue3 supports Fragments

- Vue2 has only one root node in the component.
- Vue3 can have multiple root nodes in a component.



### 3. Different API models

- The biggest difference between Vue2 and Vue3: Vue2 using Options API compared to Vue3 Composition API.



### 4. Different ways to build data

- Vue2: Put the data in the data attribute.

- Vue3: Need to use a new setup () method that fires when the component initializes.

- Use these three steps to build responsive data:

  ​	import ref or reactive from vue

  ​	Simple data types are handled with the ref() method, and complex data types are handled with reactive () .

  ​	Use the setup () method to return the responsive data so template can get it.





### 5. Different Lifecyle Hooks

- setup (): Execute beforeCreate and created before starting to create components. What is created is data and method.
- onBeforeMount (): The function performed before the component is mounted on the node.
- onMounted (): The function performed after the component is mounted.
- onBeforeUpdate (): The function performed before the component is updated.
- onUpdated (): The function executed after the component is updated.
- onBeforeUnmount (): The function performed before the component is unmounted.
- onUnmounted (): The function executed after the component is unmounted.

If the component is contained by < keep-alive > , the following two hook functions are added:

- onActivated (): The included component will have two more lifecycle hook functions. Execute when activated.
- onDeactivated (): For example, switch from component A to component B, and execute when component A disappears.

​	

### 6. Different from parent-child parameters 


- The child component receives through defineProps () , and receives the return value of this function for data operation.



### 7. Summary

Vue3 higher performance, smaller size, more conducive to reuse, more convenient code maintenance.



## Part 2

Difference between defineProperty and proxy

Vue traverses all the properties in the data during instance initialization and uses Object.defineProperty to convert all these properties to getters/setters. Moreover, it hijacks various property getters and setters, publishes messages to subscribers when the data changes, and triggers corresponding listening callbacks. There are several problems in between:

- When initializing, it is necessary to traverse all keys of the object. If the object level is deep, the performance is not good.
- The notification update process requires maintaining a large number of dep instances and watcher instances, which takes up a lot of additional memory.
- Object.defineProperty cannot listen to changes in array elements, only override array methods by hijacking them.
- Dynamic new, delete object properties can not be intercepted, can only be replaced with a specific set/delete API.
- Data structures such as Map and Set are not supported.

Vue3 uses Proxy to monitor changes in data. Proxy is a function provided in ES6, and its role is: Custom behavior for defining basic operations (such as property lookup, assignment, enumeration, function call, etc.). Compared with Object.define Property () , it has the following characteristics:

- Proxy directly proxies the entire object rather than object properties, so that only one layer of proxy can listen to all property changes under the sibling structure, including adding and deleting properties.
- Its treatment is to recursion response in the getter, the advantage is that the real access to the internal properties will become responsive, simple can be said to be responsive on demand, reduce performance consumption.
- Proxies can listen for changes in arrays.



## Part 3

Difference between Vue3 Diff algorithm and Vue2

When data changes trigger page re-rendering, a virtual DOM will be generated and the patch process will be carried out. The optimization of this process in Vue3 is as follows:

Optimization in Compilation Phase:

- Event cache: The event cache (such as: @click) can be understood as static
- Static promotion: save when creating a static node for the first time, and then directly to reuse
- Add static tags: Add static tags to nodes to optimize the Diff process

Due to the optimization of the Compilation stage, in addition to generating virtual DOM faster, it also enables Diff to skip "never changing nodes".



Diff optimizes as follows

- Vue2 is full Diff, Vue3 is static marker and non-full Diff

- Optimized comparison process using the longest increasing sequence

  

According to the data released by .org, the Vue3 update performance has improved by 1.3 to 2 times.



## Part 4

Difference between composition API and options API

1. Vue2 uses the options API.

​	(1) Advantages: easy to learn and use, each code has a clear location (for example: data in data, methods in methods)

​	(2) Disadvantages: Similar logic, not easy to reuse, especially in large projects

​	(3) Although optionsAPI can extract the same logic through mixins, it is not particularly easy to maintain

2. New to vue3 is the composition API.

​	(1) The compositionAPI organizes code based on logical functions , a function API related to put together

​	(2) Even if the project is large and has more functions, it can quickly locate function-related APIs

​	(3) greatly improve the code readability and maintainability



3. Vue3 recommends composition API, but also retains options API

​	That is, even without composition API, the writing of vue2 is fully compatible.



## Part 5

Composition API is similar to the React Hook, What are the differences between them?

From the perspective of the implementation of React Hook, React Hook is based on the order of useState calls to determine which useState the state of the next rerender comes from, so the following limitations appear:

- Hook cannot be called in loops, conditions, nested functions
- Make sure to always call the hook at the top level of your React function
- Functions such as useEffect, useMemo, etc. must manually determine dependencies

And Composition API is based on Vue's reactive system implementation, compared with React Hook's

- In the setup function, a component instantiation only calls setup once, and React Hook needs to call Hook every time it is re-rendered, making React's GC more stressful than Vue, and the performance is slower than Vue
- Compositon API calls do not need to worry about the call order, can also be used in loops, conditions, nested functions
- The reactive system automatically implements dependency collection, and then the performance optimization of the component part is completed by Vue itself, while React Hook needs to manually pass in dependencies, and must ensure the order of dependencies, so that useEffect, useMemo and other functions correctly capture dependency variables, otherwise the performance of the component will be degraded due to incorrect dependencies.

Although Compositon API looks better than React Hook, its design idea is also borrowed from React Hook.



## Part 6

Setup function

The setup () function is a new property in vue3, provided specifically for components. It provides a unified entry for us to use vue3 Composition API new features, setup function will be executed before beforeCreate , created , vue3 is also canceled the two hooks, unified with setup instead, the function is equivalent to a life cycle function, vue in the past data , methods , watch and so on all with the corresponding new api written in setup () . 



Setup ()  takes two parameters props and context . It cannot use this inside, but replaces the object currently performing context binding with a context object with four properties: attrs , slots , emit , expose

 

Inside, ref and reactive replace the previous data syntax, and the content returned can be used directly in the template, including variables and methods

```XML
<template>
  <div class="container">
    <h1 @click="say()">{{msg}}</h1>
  </div></template>

<script>export default {
  setup (props,context) {
    console.log('setup执行了')
    console.log(this)  // undefined
    // 定义数据和函数
    const msg = 'hi vue3'
    const say = () => {
      console.log(msg)
    }
    // Attribute (非响应式对象，等同于 $attrs)
    context.attrs
    // 插槽 (非响应式对象，等同于 $slots)
    context.slots
    // 触发事件 (方法，等同于 $emit)
    context.emit
    // 暴露公共 property (函数)
    context.expose

    return { msg , say}
  },
  beforeCreate() {
    console.log('beforeCreate执行了')
    console.log(this)  
  }
}
</script>复制代码
```



## Part 7

setup syntax sugar (script setup syntax)

Script setup is syntactic sugar for Compilation when using composite API in single-file components (SFC). More concise than normal script syntax

To use this syntax, add the setup attribute to the < script > Code Block:

format:

```XML
<script setup>console.log('hello script setup')
</script>复制代码
```



The top-level bindings are automatically exposed to the template, so the defined variables, functions and import imports can be used directly in the template

```XML
<template>
  <div>
    <h3>根组件</h3>
    <div>点击次数：{{ count }}</div>
    <button @click="add">点击修改</button>
  </div></template>

<script setup>import { ref } from 'vue'

const count = ref(0)
const add = () => {
  count.value++
}
</script>复制代码
```

 

When using setup syntax sugar, do not write export default {} , subcomponents only need to import to use directly, do not need to register in components as before, properties and methods do not need to return.

 

And there is no need to use async can directly use await , because this will default to the component setup to async setup

 

When using syntactic sugar, the acquisition methods of props, attrs, slots, emit, and expose are also different.

 

Versions 3.0 to 3.2 became API introduced via import: defineProps , defineEmit , useContext (deprecated in 3.2), properties of useContext {emit, attrs, slots, expose}

 

3.2 + version does not need to import, but directly calls:  defineProps , defineEmits , defineExpose , useSlots , useAttrs



## Part 8

Reactive, shallowReactive function

reactive

 Reactive () function takes a normal object and returns a reactive data object, equivalent to Vue.observable () in Vue 2.x API, the reactive transformation is "deep" - it affects all nested properties. Based on proxy, it is also very simple to use the created reactive data. After it is created, return it in setup and call it directly in the template 

 

shallowReactive

Create a reactive agent that tracks the responsiveness of its own properties shallowReactive generates non-recursion response data, only listening for changes in the first layer of data, but not performing deep reactive transformations of nested objects (exposing the original values).



## Part 9

ref、 shallowRef 、isRef、toRefs 

ref

Ref () function is used to create a reactive data object according to the given value, the return value of the ref () function call is an object, this object contains only a value attribute, only inside the setup function to access ref function needs to add .value , its purpose to create independent original value



Reactive will unpack all deep refs while maintaining ref responsiveness. When ref is assigned to a reactive property, ref is automatically unpacked

 

![img](https://p3hoy8lj8p.feishu.cn/space/api/box/stream/download/asynccode/?code=MzZjNDE3YjY1NWZjZTIzMDY4Zjg0ZDY3ZDlhNmNmNjBfd0Z3VXJaTU5mdnZDcVNNOFNDSHdvUGxVem80ZUgzUWlfVG9rZW46Ym94Y25wdTRzN1NpUHRob1NUa3ZWZWI3d3pnXzE2NzQ4OTgzNDA6MTY3NDkwMTk0MF9WNA)

 

shallowRef

A shallow form of action of ref (). shallowRef () is often used for Performance optimization of large data structures or integration with external state management systems

 

isRef

`isRef () is used to determine whether a value is an object created by ref () or not

 

toRefs 

Usage scenarios: If you deconstruct or expand a response data, you will lose its responsive features!

 

Reason: The bottom layer of vue3 is to monitor and hijack objects

Function: All internal properties of a reactive object are treated reactively

1. The reactive function of reactive/ref is assigned to the object. If the object is deconstructed or expanded, the data will lose the reactive ability.
2. Using toRefs guarantees that every property expanded by the object is responsive



## Part 10

readonly、isReadonly、shallowReadonly

readonly

Passing in a ref or reactive object and returning a read-only proxy for the original object, any nested properties inside the object are also read-only and recursion read-only.

 

isReadonly

Checks if the object is a read-only object created by readonly 

 

shallowReadonly

The shallowReadonly effect only handles reactive (shallow reactive) read-only of the outermost properties of the object, but does not perform deep read-only conversion of nested objects (exposing the original value)

What is the difference between readonly and const ?

- Const is assignment protection. A variable defined with const cannot be reassigned. But if const assigns to an object, then the contents of the object can be changed. The reason is that the variable defined by const cannot be changed. In other words, the address corresponding to the object cannot be changed
- And readonly is attribute protection and cannot reassign attributes



## Part 11

computed, watch function

computed

This function is used to create computed properties, and as in the past, the value it returns is a ref object. It can pass methods, or an object that contains set () , get () methods

 

watch

The watch function is used to listen to a specific data source and perform side effects in the callback function. By default, the callback is lazy, that is, only executed when the source data being listened for changes.

```SCSS
// 监听单个ref
const money = ref(100)
watch(money, (value, oldValue) => {
  console.log(value)
})

// 监听多个ref
const money = ref(100)
const count = ref(0)
watch([money, count], (value) => {
  console.log(value)
})

// 监听ref复杂数据
const user = ref({
  name: 'zs',
  age: 18,
})
watch(
  user,
  (value) => {
    console.log('user变化了', value)
  },
  {
    // 深度监听，，，当ref的值是一个复杂数据类型，需要深度监听
    deep: true,
    immediate: true
  }
)

// 监听对象的某个属性的变化
const user = ref({
  name: 'zs',
  age: 18,
})
watch(
  () => user.value.name,
  (value) => {
    console.log(value)
  }
)
复制代码
```

1. Difference between watch and watchEffect

 

Watch is to listen for changes in one or more values passed in; it will return new and old values when triggered; that is to say, it will not be executed for the first time, and will only be re-executed when it changes

 

watchEffect is passed in a function, it will be executed immediately, so the default will be executed once for the first time ; no need to pass in the listening content, it will automatically collect the data source in the function as a dependency , and will re-execute the function when the dependency changes, if there is no dependency will not be executed; and will not return the new and old values before and after the change

 

The common point is that watch and watchEffect share the following four behaviors:

- Stop listening : Components automatically stop listening when they are uninstalled
- Clear side effects : onInvalidate is passed as the third argument to the callback
- Side effect refresh timing : The reactive system will cache the side effect function and refresh it asynchronously to avoid repeated calls caused by multiple state changes in the same tick
- Listener debugging : Debug with onTrack and onTrigger in development mode



## Part 12

Vue3's life cycle

Basically added on to the Vue2 lifecycle hook function name; renamed beforeDestory and destoryed to onBeforeUnmount and onUnmounted; then replaced the two hook functions beforeCreate and created with setup; added two Development Environment hooks for debugging

 

![img](https://p3hoy8lj8p.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDdlMWIwNjQ5MzBhZTExMmNmYjAzZmMxZDFiMGY3MWRfbHRSUW9lTGtNaUszWXpNb0dzMTk5WEFnb3FJQ0RLa0lfVG9rZW46Ym94Y25Od2p3Y3FEZ3FYa2ZYVWg3RGhESWpOXzE2NzQ4OTg0OTA6MTY3NDkwMjA5MF9WNA)



## Part 13

Difference between Vite and Webpack

- All modern packaging tools
- The startup method is different. Vite does not need to be packaged at startup, so there is no need to analyze the dependencies between modules and modules, and there is no need for Compilation. This method is similar to when we use a UI framework, we can load it on demand. Similarly, vite is also this mechanism. When the browser requests a module, it compiles the content of the module as needed. On-demand dynamic Compilation can reduce Compilation time. When the project is more complex and the modules are more, vite is obviously better than webpack.
- Hot Module Replacement is more efficient. When you change a module, you only need to ask the browser to re-request the module, and you don't need to Compilation all the modules and modules that the module depends on like webpack.

