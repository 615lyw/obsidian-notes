---
date created: 2022-05-18, 21:14:59
date modified: 2022-05-22, 10:18:49
---

# Meta

- parent:: [[Content-Type]]
- siblings::
- child::
- refs: [MIME 类型 - HTTP | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types)

---

浏览器使用 MIME 类型（而不是文件扩展名）识别下载的文件的格式，再决定如何处理。（同 Windows 根据文件后缀名识别文件类型）

语法结构：`type/subtype`。MIME 类型大小写不敏感，但传统写法为小写。

对于 text 文件类型若没有特定的 subtype，就使用 `text/plain`。类似的，二进制文件没有特定或已知的 subtype，即使用 `application/octet-stream`，含义：未知的应用程序文件。

即从服务器下载的通用软件包就是这个类型，浏览器就像对待 [[Content-Disposition]] `attachment` 一样。
