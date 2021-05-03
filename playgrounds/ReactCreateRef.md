# ReactCreateRef

> `packages/react/src/ReactCreateRef.js`

本质就是调用 `React.createRef()` 方法, 它返回一个对象, 对象里有个 `current` 属性.

```ts
const refObject = {
  current: null,
};
```

很常用的功能, 使用如下:

```ts
const $el = createRef<HTMLInputElement>()

// 使文本框对焦
$el.current?.focus()
```
