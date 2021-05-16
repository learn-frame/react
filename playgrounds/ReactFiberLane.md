# ReactFiberLane

为什么使用 Lane 来代替 expirationTime, 可参考这个 [pr](https://github.com/facebook/react/pull/18796)

- 利用了位掩码的特性, 在频繁的时候占用内存少, 计算速度快
- Lanes(一组 Lane) 和 Lane 都是 32 位二进制数字, 可以通过 `lanes |= lane` 的方式来 merge, 可以用 `a &= ~b` 删除, 操作方便
- 处理一组任务时, 如果是 expirationTime, 要判断一个范围, 而 Lane 直接用位运算搞定

    ```ts
    taskPriority <= highestPriorityInRange &&
    taskPriority >= lowestPriorityInRange
    ```

## 复合权限设计

- a |= b; // 添加属性
- a &= ~b; // 删除属性
- a & b === b // 判读是否有 b 属性

## 判断优先级的小技巧

```ts
// 分离出最高优先级
export function getHighestPriorityLane(lanes: Lanes): Lane {
  return lanes & -lanes;
}
```

```ts
// 获取最小优先级
// clz32(lanes) 返回一个数字在转换成 32 无符号整形数字的二进制形式后, 前导 0 的个数
// 后续通过 1 << 31 - clz32(lanes) 来分离最左边的 1, 从而得到最小优先级
function pickArbitraryLaneIndex(lanes: Lanes) {
  return 31 - clz32(lanes);
}
```
