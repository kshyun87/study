In this kata, you must create a digital root function.

A digital root is the recursive sum of all the digits in a number. Given n, take the sum of the digits of n. If that value has two digits, continue reducing in this way until a single-digit number is produced. This is only applicable to the natural numbers.

Here's how it works (Ruby example given):

## 나의 답
```python
def digital_root(n):
    # ...
    a = str(n)
    b = 0
    for each in a:
        b+=int(each)
    if b > 9:
        return digital_root(b)
    else:
        return b
```

## 추천 답
```python
def digital_root(n):
    return n if n < 10 else digital_root(sum(map(int,str(n))))
```
이 답변이 베스트인 이유는 아마도 재귀적 함수 호출을 이용해서 인듯 하다.

## 가장 좋다고 한 답
```python
def digital_root(n):
    return n%9 or n and 9
```
가장 코드가 간결하다.
이 코드는 삼항연산식이다.
보통 컴퓨터 언어에서는 and, or, not은 보통 조건을 비교하기 위해서 사용한다.
(not 아닐 수도 있음)
그러나!
파이썬에서는 삼항조건식?삼항연산식?이다.

각 의미는 다음과 같습니다.

|조건    |의미                                 |
|-------|-------------------------------------|
|a and b|if a is false, then a, else b        |
|a or b |if a is false, then b, else a        |
|a not b|if a is false, then true, else false |

위의 코드는 이 삼항 연산식을 이용해서 작성한 코드기에 간단하다.
코드를 다시 살펴보면,
A or B and C 로 간단하게 표현하면,
A가 참이면 A를 반환하고 끝,
A가 거짓이면 (B and C)를 반환하므로 다시 연산해야한다.
B and C에서 B가 참이면 C를 거짓이면 B를 반환한다.

만약 n이 10이면 9로 나누면 나머지가 1이므로 1은 조건이 참이므로 1이 반환된다.
하지만 n이 9이면 나머지가 0이므로 조건은 거짓이 된다. 그러므로 (n and 9)가 반환된다. 여기서 9
