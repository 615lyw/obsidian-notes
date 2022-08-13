---
date created: 2022-05-18, 20:31:41
date modified: 2022-07-03, 16:01:43
---

# Meta

- alias:
- parent :: [[URI VS URL]]
- siblings ::
- child ::
- refs: https://datatracker.ietf.org/doc/html/rfc2616#page-17

---

在 HTTP 上下文语境中 URL 和 URI 含义？

之前理解的 URI 是 URL 和 URN 的超集。在 [[MOC HTTP|HTTP]] 语境下定义又不一样了。

> As far as HTTP is concerned, Uniform Resource Identifiers are simply formatted strings which identify--via name, location, or any other characteristic--a resource.
>
> `http_URL = "http:" "//" host [ ":" port ] [ abs_path [ "?" query ]]`

其中 abs_path 部分叫 "Request-URI"。
