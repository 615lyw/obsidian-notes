# RFC6266 Content-Disposition in HTTP 翻译

Content-Disposition 虽广泛使用，但并不是 HTTP/1.1 标准。

RFC6266 仅定义了在 header 中的 Content-Disposition，没有定义在 `multipart/form-data`。

3 错误处理

4 Content-Disposition response header 字段定义了浏览器如何处理响应资源。

4.1 Grammar 定义了书写语法。

4.2 定义了 `attachment` 和 `inline` 对应的 UA 的处理行为。

4.3 叙述了 filename 和 filename* 的区别。

filename* 可以使用 [[ISO-8859-1]] 和 [[UTF-8]] 编码。

5 叙述了几个例子。

filename 的值若包含空格，可以使用双引号括起来。

Appendix D 对生成 Content-Disposition 字段给出了一些建议。

---

综上所述：

生成 filename 和 filename* 的最佳实践：

1. 当使用 ASCII 码足够表达时，只需要使用 filename
2. 如果超出了 [[ISO-8859-1]] 字符集，使用 filename*，且 filename* 最好使用 [[UTF-8]] 编码，为了最大兼容性，也需要加上 filename 字段作为备案
3. filename 和 filename* 同时出现时，先写 filename，后写 filename*

## 参考

[RFC 6266 - Use of the Content-Disposition Header Field in the Hypertext Transfer Protocol (HTTP)](https://datatracker.ietf.org/doc/html/rfc6266)
