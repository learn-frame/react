# ReactElement

> `packages/react/src/ReactElement.js`

## New JSX transform

旧的 jsx 会被编译成 `React.createElement`, 因此你不得不在 jsx 文件中引入 `React`, 新的 babel 编译包会使用 `import jsx from 'react/jsx-runtime'`.

前者第一个参数是 type(组件是引用, html 标签是字符串), 第二个参数是 config(props), 从第三个起是 children. 后者将 children 也收敛到第二个参数 config 中.

```tsx
import {FC} from 'react';

interface Props {
    disabled: boolean
    text: string
    isLoading: boolean
}

const Button: FC<Props> = ({disabled, text,isLoading}) => {
  return <button disabled={disabled}>
      {isLoading && <span>loading...</span>}
      <span>{text}</span>
  </button>;
};

export default Button;
```

```js
'use strict';

Object.defineProperty(exports, '__esModule', {
  value: true,
});
exports.default = void 0;

const ButtonBase = () => /*#__PURE__*/ React.createElement('button', null);

const Button = ({disabled, text, isLoading, type}) => {
  return /*#__PURE__*/ React.createElement(
    ButtonBase,
    {
      disabled: disabled,
      type: type,
    },
    isLoading && /*#__PURE__*/ React.createElement('span', null, 'loading...'),
    /*#__PURE__*/ React.createElement('span', null, text)
  );
};

var _default = Button;
exports.default = _default;

const profile = ({props1, props2}) =>
  /*#__PURE__*/ _jsxs(_Fragment, {
    children: [
      /*#__PURE__*/ _jsx('img', {
        src: 'avatar.png',
        className: 'profile',
      }),
      /*#__PURE__*/ _jsx('h3', {
        children: /*#__PURE__*/ _jsx('p', {
          children: [user.firstName, user.lastName].join(' '),
        }),
      }),
    ],
  });
```

## hasOwnProperty

JavaScript 未对 hasOwnProperty 做保护, 因此它可以成为一个对象的 key, 如:

```ts
const o = {
    hasOwnProperty() {
        return false
    },
    name: 'Yancey'
}
```

因此为防止 hasOwnProperty 被污染, 可以使用:

```ts
// 判断对象 o 是否有属性 name
Object.prototype.hasOwnProperty.call(o, 'name')
```
