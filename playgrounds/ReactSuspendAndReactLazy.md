# ReactSuspendAndReactLazy

这俩按 React core team 的目标要配合着 concurrent 一起玩. 不过目前只能做一些基本的异步加载组件的工作. 一般我们按照路由进行 code splitting, 亲测效果不如 `Loadable Components` 这个库.

## Suspense

对于 `Suspense`, 目前在源码里也仅仅是暴露出两个 Symbol, 分别是 `REACT_SUSPENSE_TYPE` 和 `REACT_SUSPENSE_LIST_TYPE`.

## Lazy

初始化方法 `lazyInitializer` 接收 `paylaod` 对象, 而 `payload._result` 的返回值是一个 thenale 对象. 通过执行这个 thenale 对象的 then 方法, 这里类似于 Promise, 即状态会从 `pending -> resolved`, 或者 `pending -> rejected`, 获取异步结果.

```ts
// 存储异步结果, 一般我们用于异步加载组件.
const payload: Payload<T> = {
  _status: -1,
  _result: ctor,
};

const lazyType: LazyComponent<T, Payload<T>> = {
  $$typeof: REACT_LAZY_TYPE,
  _payload: payload,
  _init: lazyInitializer,
};
```
