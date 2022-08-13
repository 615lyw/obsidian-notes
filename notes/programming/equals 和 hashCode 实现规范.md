---
date created: 2022-07-03, 16:02:47
date modified: 2022-07-05, 11:54:35
---

# Meta

- alias:
- parent ::
- siblings ::
- child ::
- refs:

---

# equals 实现规范

1. **对称性**：如果 `x.equals(y)` 返回是 "true"，那么 `y.equals(x)` 也应该返回是 "true"
2. **反射性**：`x.equals(x)` 必须返回是 "true"
3. **类推性**：如果 `x.equals(y)` 返回是 "true"，而且 `y.equals(z)` 返回是 "true"，那么 `z.equals(x)` 也应该返回是 "true"
4. **一致性**：如果 `x.equals(y)` 返回是 "true"，只要 x 和 y 内容一直不变，不管你重复 `x.equals(y)` 多少次，返回都是 "true"
5. **非空性**：`x.equals(null)`，永远返回是 "false"； `x.equals(和 x不同类型的对象)` 永远返回是 "false"

# hashCode 实现规范

1. 两个对象相等时（equals 返回 true），hashCode 值一定相等
2. 两个对象不等时（equals 返回 false），hashCode 值绝大概率不同，小概率相同

只要 hashCode 不同，必为两个不同的对象。

# 总结

hashCode 不同时，必为两个不同的对象。

equals 是判断对象是否相等的最终依据，hashCode 是**快速判断**对象是否相等的依据，但由于存在哈希冲突，当 hashCode 相同时需要再通过 equals 来最终断定对象是否相等。
