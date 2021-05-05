# ReactContext

> `packages/react/src/ReactContext.js`

## 使用方法

```tsx
const context = createContext('light')
const {Provider, Consumer} = context

<Provider value='dark'>
  <ChildComponet />
</Provider>
```

```tsx
const ChildComponet: FC = () => {
  // 可以通过传统的 Consumer 来订阅 context
  const ThemeConsumer = ThemeContext.Consumer

  // 也可以用 Hooks
  const context = useContext(ThemeContext)

  // 下面不管是 context 还是 value, 都展示为 'dark' 字符串
  return (
    <ThemeConsumer>
      {(value) => {
        return (
          <>
            <div>uses Consumer component: {value}</div>
            <div>uses useContext hooks: {context}</div>
          </>
        )
      }}
    </ThemeConsumer>
  )
}
```

## 源码解析