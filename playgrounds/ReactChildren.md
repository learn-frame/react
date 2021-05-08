# ReactChildren

以 `map` 方法为例, 它先判断 children 是否为单个(字符串, 数字, 一个 ReactNode), 如果是单个, 包装成数组, 然后根据回调函数递归即可; 如果 children 是多个, 那就一个一个根据回调函数深度递归即可.

注意:

- Children 可以是 ReactNode, 数字, 字符串, Set, 数组
- undefined 和 boolean 类型被转成了 null
- 但不能是 Map, Functions, 普通对象, 其中如果是「普通对象」, App 直接 fatal, 前两个会给予控制台警告
