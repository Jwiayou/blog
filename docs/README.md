## 基础知识

1. 使用`@`代替 `v-on`， 使用 `:`代替 `v-bind`

2. 只有在实例被创建时，`data`存在的属性才是**响应式**的。

3. 还是由于 JavaScript 的限制，Vue 不能检测对象属性的添加或删除,需要使用 :
```JavaScript
   Vue.set(vm.userProfile, 'age', 27)
   vm.$set(vm.userProfile, 'age', 27)

   // 设置对象数据上多个属性
   vm.userProfile = Object.assign({}, vm.userProfile, {
     age: 27,
     favoriteColor: 'Vue Green'
   })
```

4. 带有`v-once`指令标签，插入的值不会更改。

5. 插入`html`内容需要使用 `v-html` 指令

6. 修饰符 `.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`

7. `v-show` 不可以在`template` 元素上使用。

8. `v-for` 中接受第二个参数为 `index`。如果循环的是一个`object`,则第二个参数为`key`,第三个为`index`

9. **计算属性**

    1. 模板中放入太多的逻辑会让模板过重且难以维护，过多的逻辑放入计算属性.
    2. 计算属性具有缓存机制，只要依赖属性没有发生改变就不会触发。
    3. 计算属性可以有多个依赖对象，属性，或者说是数据。
    4. 计算属性默认是 `getter` ， 但是也有`setter`。需要主动声明。

10. **计算属性**和 **监听属性**的作用范围：
     1. `computed`是根据一个或者多个数据返回一个新的数据，
     2. 而 `watch` 是监听一个固定数据更改后执行的动作`(function)`，范围更广。
      没有局限。

11，`v-model` 是 `v-bind:value` 和 `v-on:input` 的间写。
> **注意:** `input` 事件是实时的监听数据改变触发的。而 `change` 事件是在焦点离开元素后才触发。<br/>
> 可以在使用 `v-model.lazy` 惰性修饰符将 `input` 事件修改成 `change` 事件

## 生命钩子


#### 1. beforeCreate
> 在实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用

#### 2. created
> 实例己经创建完成之后调用。在一步，实例完成了一下配置：数据观测（data observer）,属性和方法的元算，watch/event事件回调。然而，挂载阶段还没有开始。$el属性目前还可见

#### 3. beforeMount
> 在挂载之前调用，相关render首次调用。

#### 4. mounted
> `el`被新创建的 `vm.$el` 替换，并挂载到实例上后调用改钩子。
>> **注意：** mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick 替换掉 mounted

#### 5. beforeUpdate
> 数据更新时调用，发生在虚拟DOM 重新熏染和打补丁之前

#### 6. updated
> 由于数据更改导致的虚拟DOM的重新渲染和打补丁，在这之后会调用改钩子
>> 当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。

>> 该钩子在服务器端渲染期间不被调用。

#### 7. beforeDestroy
> 实例销毁之前调用。在这一步，实例仍然完全可用

#### 8. destroyed
> Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
>> 该钩子在服务器端渲染期间不被调用。


#### 9. Vue.nextTick
> Vue 实现响应式并不是数据发生变化之后 DOM 立即变化。更新后的 DOM 可以在 `nextTick` 钩子中获取。

