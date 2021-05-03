# ReactForwardRef

> `packages/react/src/ReactForwardRef.js`

## 使用场景

### 场景一

下面的例子中, 如果没有 `forwardRef`, 那么给 `FancyButton` 组件施加的 ref 只能获取该组件的实例, 而我们本意是希望获取该组件下 `button` 标签的实例, 因此需要使用 `forwardRef` 转发出来.

```tsx
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// 你可以直接获取 DOM button 的 ref：
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

### 场景二

第二个场景就是在高阶组件中, ref 指向的不是被**包装的组件**, 而是**外面这个高阶组件壳**, 如果你想将 ref 作用于内部组件, 需要使用 `forwardRef`.


```tsx
// 高阶组件
function logProps(WrappedComponent) {
  class LogProps extends React.Component {
    componentDidUpdate(prevProps) {
      console.log('old props:', prevProps);
      console.log('new props:', this.props);
    }

    render() {
      return <WrappedComponent {...this.props} />;
    }
  }

  return LogProps;
}
```

```tsx
// FancyButton 组件
class FancyButton extends React.Component {
  render() {
    return <button />;
  }
}

// 我们导出的是 LogProps, 而不是 FancyButton.
// 虽然它也会渲染一个 FancyButton.
export default logProps(FancyButton);
```

```tsx
import FancyButton from './FancyButton';

const ref = React.createRef();

// 我们导入的 FancyButton 组件是高阶组件 LogProps.
// 尽管渲染结果将是一样的, 
// 但我们的 ref 将指向 LogProps 而不是内部的 FancyButton 组件.
// 这意味着我们不能调用例如 ref.current.focus() 这样的方法
<FancyButton
  label="Click Me"
  handleClick={handleClick}
  ref={ref}
/>;
```

```tsx
function logProps(WrappedComponent) {
  class LogProps extends React.Component {
    componentDidUpdate(prevProps) {
      console.log('old props:', prevProps);
      console.log('new props:', this.props);
    }

    render() {
      const {forwardedRef, ...rest} = this.props;

      // 在开发环境中可以为该组件提供一个更有用的显示名
      const name = Component.displayName || Component.name;
      forwardRef.displayName = `logProps(${name})`;

      // 将自定义的 prop 属性 "forwardedRef" 定义为 ref
      return <WrappedComponent ref={forwardedRef} {...rest} />;
    }
  }

  // 注意 React.forwardRef 回调的第二个参数 "ref".
  // 我们可以将其作为常规 prop 属性传递给 LogProps, 例如 "forwardedRef"
  // 然后它就可以被挂载到被 LogProps 包裹的子组件上.
  return React.forwardRef((props, ref) => {
    return <LogProps {...props} forwardedRef={ref} />;
  });
}
```

## 源码

本质来讲就是返回下面这个对象, 对于 `logProps(FancyButton)` 这个整体, 它的 `$$typeof` 是 `REACT_FORWARD_REF_TYPE`.

```ts
const elementType = {
  $$typeof: REACT_FORWARD_REF_TYPE,
  render,
};
```
