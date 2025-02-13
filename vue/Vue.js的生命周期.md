Vue. Js 的生命周期指的是从实例创建到销毁的过程。在这个过程中，Vue 实例会经历一系列的初始化步骤，包括数据观察、编译模板、挂载 DOM、更新 DOM 和销毁实例。每个阶段都有相应的生命周期钩子，可以在特定的时间点执行代码。以下是 Vue. Js 生命周期的各个阶段及其钩子函数：

1. **创建阶段**
   - `beforeCreate`：实例刚创建完成，数据观测和事件配置都未完成。此时无法访问 `data` 和 `methods` 中的内容。
   - `created`：实例创建完成，数据观测和事件配置完成，但未挂载到 DOM 上。此时可以访问 `data` 和 `methods` 中的内容。

2. **挂载阶段**
   - `beforeMount`：在挂载之前调用，相关的 `render` 函数首次被调用。此时虚拟 DOM 已创建，但尚未挂载到 DOM 上。
   - `mounted`：实例挂载到 DOM 上，`el` 被新创建的 `vm.$el` 替换。此时可以进行 DOM 相关的操作。

3. **更新阶段**
   - `beforeUpdate`：在数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。此时可以在更新之前访问现有的 DOM。
   - `updated`：由于数据更改导致的虚拟 DOM 重新渲染和打补丁完成。此时可以访问更新后的 DOM。

4. **销毁阶段**
   - `beforeDestroy`：实例销毁之前调用，此时实例仍然完全可用。可以在这里执行清理任务，例如清理计时器。
   - `destroyed`：实例销毁后调用，此时所有的事件监听器被移除，所有的子实例也被销毁。

### 生命周期图示

```plaintext
beforeCreate -> created -> beforeMount -> mounted -> beforeUpdate -> updated -> beforeDestroy -> destroyed
```

### 代码示例

```javascript
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  },
  beforeCreate() {
    console.log('beforeCreate');
  },
  created() {
    console.log('created');
  },
  beforeMount() {
    console.log('beforeMount');
  },
  mounted() {
    console.log('mounted');
  },
  beforeUpdate() {
    console.log('beforeUpdate');
  },
  updated() {
    console.log('updated');
  },
  beforeDestroy() {
    console.log('beforeDestroy');
  },
  destroyed() {
    console.log('destroyed');
  }
});
```

通过这些生命周期钩子，你可以在 Vue 实例的不同阶段执行特定的逻辑，以便更好地控制应用程序的行为和优化性能。