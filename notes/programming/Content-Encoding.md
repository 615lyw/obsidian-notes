# Content-Encoding

此 header 类型为 [[Representation header type]]。

告诉接收者如何 decode http msg body 以获取原始数据。主要使用目的是为了压缩 msg data。

[[Content-Type]] 指定消息的原始类型。如果原始类型已被压缩，则无需指定 Content-Encoding。