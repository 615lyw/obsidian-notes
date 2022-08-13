---
date created: 2022-05-20, 16:27:16
date modified: 2022-08-11, 12:25:17
---

# Meta

- parents: [[Representation header type]]
- refs:
    - [Content-Disposition in HTTP-RFC](https://datatracker.ietf.org/doc/html/rfc6266)
    - [[RFC6266 Content-Disposition in HTTP 翻译]]
    - [Content-Disposition-MDN Doc](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition)

---

# Content-Disposition

`内容-处理方式`

## As a response header for the main body

一般作为服务端返回给客户端的 header。

作用：**告知浏览器处理资源的行为**，若为 `attachment` 则表示浏览器应该把资源作为附件保存在本地；若为 `inline` （默认）则表示直接打开一个页面显示文件内容。

Disposition Type:（大小写不敏感）

- `attachment`
- `inline`

Disposition Parameter:

- `filename`
- `filename*`

生成 `filename` 和 `filename*` 的最佳实践：

1. 未超出 ISO-8859-1 字符集时只需要使用 `filename`
2. 超出了 ISO-8859-1 字符集（又称 Latin-1），使用 `filename*`，且 `filename*` 最好使用 [[UTF-8]] 编码（编码格式参考 [RFC 5987 - Character Set and Language Encoding for Hypertext Transfer Protocol (HTTP) Header Field Parameters](https://datatracker.ietf.org/doc/html/rfc5987#section-3.2) ），为了最大兼容性，也需要加上 `filename` 字段作为备案
3. `filename` 和 `filename*` 同时出现时，先写 `filename`，后写 `filename*`

当两者同时出现时，[[UA]] 优先使用 `filename*`。

示例：

```
Content-Disposition: inline
Content-Disposition: attachment
Content-Disposition: attachment; filename="filename.jpg"; filename*="UTF-8''%E4%B8%AD%E6%96%87%E6%96%87%E4%BB%B6%E5%90%8D.txt"
```

## As a header for a multipart body

示例：

![[Pasted image 20220318162114.png]]

```
Content-Disposition: form-data; name="fieldName"
Content-Disposition: form-data; name="fieldName"; filename="filename.jpg"
```

在 `multipart/form-data` body 中每一个子部分必须有 `Content-Disposition` 部分，且第一个参数必须为 `form-data`，还必需 `name` 参数表示 Field name。
