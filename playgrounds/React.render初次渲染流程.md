# React.render 初次渲染流程

![uu1uwy0bbb-1620795569737](https://static.yancey.app/uu1uwy0bbb-1620795569737)

在这个过程中, `document.getElementById('root')` 被挂载了 4 个跟 React 相关的属性:

- __reactEvents: 一个 Set 集合, 有 `cancel__capture`, `click_bubble` 等等.

- _reactListening: boolean, 有一个方法叫做 `listenToAllSupportedEvents`, 也就是如果事件是被支持的, 这里就是 true.

- _reactRootContainer: 这里就是 FiberRoot 类型了, 直接看图吧.