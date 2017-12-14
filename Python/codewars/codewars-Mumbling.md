This time no story, no theory. The examples below show you how to write function accum:

Examples:
```python
accum("abcd")    # "A-Bb-Ccc-Dddd"
accum("RqaEzty") # "R-Qq-Aaa-Eeee-Zzzzz-Tttttt-Yyyyyyy"
accum("cwAt")    # "C-Ww-Aaa-Tttt"
```
The parameter of accum is a string which includes only letters from a..z and A..Z.


```python
def accum(s):
    # your code
    n = 1
    v =''
    for each in s:
        v = v + each.upper() + each.lower() * (n-1) +'-'
        n+=1
    return v[:-1]
```

```python
def accum(s):
    return '-'.join(c.upper() + c.lower() * i for i, c in enumerate(s))
```
