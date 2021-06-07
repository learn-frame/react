# 渲染

React 把更新分成两部分, 第一部分是 Render, 第二部分是 Commit. 前者可以是异步可中断的, 而后者会同步一步到位. 这篇文档只说 Render.

Render 阶段的入口分别为 `performSyncWorkOnRoot` 和 `performConcurrentWorkOnRoot`, 前者是同步后者是异步, 区别仅仅在是否 `shouldYield`.

```ts
// performSyncWorkOnRoot 会调用该方法
function workLoopSync() {
  while (workInProgress !== null) {
    performUnitOfWork(workInProgress);
  }
}

// performConcurrentWorkOnRoot 会调用该方法
function workLoopConcurrent() {
  while (workInProgress !== null && !shouldYield()) {
    performUnitOfWork(workInProgress);
  }
}
```

Render 阶段本身又分为两种, 分别是 `performUnitOfWork` 和 `completeUnitOfWork`. 前者通过 `beginWork` 方法(给变更打上标记), 类似"递"的过程, 从根 Fiber 往下完成深度优先遍历. 需要注意的是, 一开始它只完成一个分叉, 比如下面这个图, 它到了 input, 就无叶子节点了, 然后通过 `completeWork` 方法(将标记收集上报)进行"归"的过程, 即先回到 Input, 此时发现 Input 有个 sibling 是 List, 那就停止"归", 继续对 List 进行"递".

总之就是遍历所有的 Fiber 节点完成更新, 再回到了根 Fiber, 就可以进行下一步的 Commit 阶段了.

```ts
RootFiber → Fiber
              ↓
             App
              ↓
            Input → List
              ↓      ↓
            input    li → li → li → li
```
