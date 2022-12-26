---
date created: 2022-12-23, 16:40:15
date modified: 2022-12-23, 16:41:49
---

```java
List<Integer> listA = Arrays.asList(new Integer[]{1, 2});
List<Integer> listB = Arrays.asList(new Integer[]{3, 4});

List<Integer> res = Stream.of(listA, listB)
                          .flatMap(Collection::stream)
                          .collect(Collectors.toList());
```
