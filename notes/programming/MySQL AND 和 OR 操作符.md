Meta:

- parents: [[MySQL SQL 书写]]
- status: #Update
- refs:
- create time: 2022-05-19 08:56:04
- update time:  2022-05-19 08:56:04

---

AND 只要有一个 0 结果则为 0:

- 1 AND NULL  -> NULL
- 0 AND NULL  -> 0

OR 只有有一个 1 结果则为 1:

- 0 OR NULL  -> NULL
- 1 OR NULL  -> 1
