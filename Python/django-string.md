# 장고
# 문자열 추출
## codewars의 문제 풀이
## 문제
<p>Return the number (count) of vowels in the given string.<br>

We will consider a, e, i, o, and u as vowels for this Kata.<br>

The input string will only consist of lower case letters and/or spaces.</p>
## 번역
문자열의 모음의 개수를 알고 싶은데, 입력되는 문자열은 소문자와 빈칸만 고려해서 설계해봐라.

위 문제를 다음 getCount함수를 구현하여 해결해야한다.

```python
def getCount(inputStr):
    num_vowels = 0
    # your code here

    return num_vowels
```
아래 테스트 코드가 동작하면서 테스트가 정상적인지를 확인해야한다.
```python
test.assert_equals(getCount("abracadabra"), 5)
```

## 일반적인 방법
```python
def getCount(inputStr):
    num_vowels = 0
    for each in inputStr:
        if each in ['a','e','i','o','u']:
            num_vowels+=1
    return num_vowels
```
일단 전달 받은 문자열을 한개씩 쪼개서 봐야하니까 for문을 이용한다.<br>
for문을 이용해서 뽑아낸 문자 하나를 if문을 통해 모음에 들어 있는지 비교한다.<br>
있는 경우 num_vowels를 1증가시키고 마무리 되면 그 값을 반환한다.<br>

위 코드를 수행하면 정상적으로 동작된다.<br>
그러나 최적화된 코드일까?<br>
추천받은 답을 확인하자.

## 정답(최고의 답)
```python
def getCount(inputStr):
    return sum(1 for let in inputStr if let in "aeiouAEIOU")
```
일단 우린 num_vowels를 버려야 한다.<br>
굳이 필요없다는 말이다.<br>
for와 if를 한줄로 쓸수 있다.<br>
여기서 in 안에 값이 배열이어야 할 것이라 생각했지만 문자열 그대로 나열하더라도 상관없다.<br>
저 문자열에 포함되는 값이 있는지를 검사하기 때문이다.<br>
sum()으로 조건을 만족할때마다 1이 반환되어 나열된 배열의 값을 합쳐서 반환하게 된다.<br>
코드가 한줄로 깔끔하게 줄었다.
