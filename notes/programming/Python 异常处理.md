# 异常体系

https://docs.python.org/3/library/exceptions.html#exception-hierarchy

# 语法

例子：

```python
try:
    print('try...')
    r = 10 / int('a')
    print('result:', r)
except ValueError as e:
    print('ValueError:', e)
except ZeroDivisionError as e:
    print('ZeroDivisionError:', e)
finally:
    print('finally...')
```
