---
date created: 2022-07-04, 10:19:44
date modified: 2022-08-11, 12:34:53
---

# Meta

- alias:
- parent :: [[MOC HTTP]]
- siblings ::
- child ::
- refs: [POST - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)

---

# POST

前端发送 POST 请求有两种方式：

- 通过 HTML `<form>` 标签 `method='POST'`
- 通过 [[AJAX]] 发送请求

## 通过 form 标签发请求

`<form>` 标签发 POST 请求时，默认的 `Content-Type` 为 `application/x-www-form-urlencoded` 类型。

特点为：在 Body 中的 key-value 键值对以 `&` 作为分隔符，非英文字母和数字通过 [[Percent-encoding]] 编码。

例如：

```
POST /test HTTP/1.1
Host: foo.example
Content-Type: application/x-www-form-urlencoded
Content-Length: 27

field1=value1&field2=value2
```

`<form>` 标签发 POST 请求时，还可以指定为 `multipart/form-data` 类型。

**`application/x-www-form-urlencoded` VS `multipart/form-data`**

- 两者均为了在一个请求中传输多个部分
- `form-urlencoded` 仅传输多个简单的 key-value 键值对，因为会对 key-value 键值对中非字母和数字的字符进行 [[Percent-encoding]]，故无法传输一般的二进制流，故需要传输二进制信息（文件或图片等）需要使用 `form-data`
