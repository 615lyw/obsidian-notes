Meta:

- parents: [[MySQL SQL 书写]]
- status: #New 
- refs: 
- create time: 2022-05-18 22:50:20
- update time:  2022-05-18 22:50:20

---

- 排序 ORDER BY
    - 可以对一个或多个列排序。多列排序规则：先以第一列排序，第一列相同再以第二列排序
    - 指定排序顺序
        - 默认升序 ASC 排列，DESC 指定降序排列
        - ORDER BY prod_price DESC, prod_name
        表示先按 prod_price 降序排列，再按 prod_name 升序排列
        - 想指定多列降序，需在每列后指定 DESC
