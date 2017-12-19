## 문제
Given an array of integers your solution should find the smallest integer.

For example:

Given [34, 15, 88, 2] your solution will return 2
Given [34, -345, -1, 100] your solution will return -345
You can assume, for the purpose of this kata, that the supplied array will not be empty.

## 나의 답
```python
def findSmallestInt(arr):
    #Code here
    return min(arr)
```




## 추천 답
```python
findSmallestInt=min  
```

```python
def findSmallestInt(arr):
    smallest = []
    for i in range(0,len(arr)):
        if (arr[i] < smallest):
            smallest = arr[i]
    return smallest
```
