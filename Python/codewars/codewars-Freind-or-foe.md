## 문제
Make a program that filters a list of strings and returns a list with only your friends name in it.

If a name has exactly 4 letters in it, you can be sure that it has to be a friend of yours! Otherwise, you can be sure he's not...

Ex: Input = ["Ryan", "Kieran", "Jason", "Yous"], Output = ["Ryan", "Yous"]

Note: keep the original order of the names in the output.
## 해설
주어진 이름들 중에서 당신의 친구 이름만 뽑아내고 싶다.
당신의 친구들의 이름은 4글자로 되어 있으며 다른 이름은 친구가 아니다.
주의 : 주어진 이름의 순서는 지켜야 한다.
즉, 리스트안에 있는 문자열 중 4글자로 된 것만 뽑아서 반환하는 함수를 만들라는 것.

## 내가 만든 것
```python
def friend(x):
    #Code
    return [each for each in x if len(each) == 4]
```

## 배스트 답
```python
def friend(x):
    return [f for f in x if len(f) == 4]
```

## 정리
이번엔 베스트 답과 일치.
이제 for +if를 간단하게 쓸 수 있는 것 같다.
