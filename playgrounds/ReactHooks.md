# ReactHooks

hooks 在入口处很简单, `resolveDispatcher` 方法就是返回 `ReactCurrentDispatcher.current`, 即 `dispatcher`, 最后返回指定的 hooks.

```ts
const ReactCurrentDispatcher = {
  /**
   * @internal
   * @type {ReactComponent}
   */
  current: (null: null | Dispatcher),
};
```

```ts
export function useEffect(
  create: () => (() => void) | void,
  deps: Array<mixed> | void | null,
): void {
  const dispatcher = resolveDispatcher();
  return dispatcher.useEffect(create, deps);
}
```
