## 문제
Complete the method/function so that it converts dash/underscore delimited words into camel casing. The first word within the output should be capitalized only if the original word was capitalized.

```python
# 예
# returns "theStealthWarrior"
to_camel_case("the-stealth-warrior")
to_camel_case("The_Stealth_Warrior")
```
### 문제 해설
'-'또는 "\_"가 포함된 문자열을 카멜식 표기법으로 변환하는 것이다.
카멜식 표기는 낙타의 등과 같은 문자열로 단어의 앞 글자를 대문자로 표기하는 것이다.



## 나의 답
```python
def to_camel_case(text):
    if not text:
        return ''

    check =0
    result = ''
    for each in text:
        if '-' == each or '_' == each:
            check = 1
        else:
            if check == 1:
                result += each.upper()
                check = 0
            else:
                result += each                

    return result   
```
답을 만들고 나니 매우 긴 함수가 되었다.


## 추천 답
```python
def to_camel_case(s):
    return s[0] + s.title().translate({ord('-'):None, ord('_'):None})[1:] if s else s
```
추천답을 보니 평소에 사용하지 않았던 함수들을 사용한 것을 보았다.
title() 메소드와 translate()메소드이다.
title은 대문자를 만들기에 좋은 메소드다.

일단 대문자로 변환하는 함수를 정리해본다.
* upper()
* capitalize()
* title()

이렇게 세가지 방법이 있고 각기 다른 변환을 보여준다.
### upper()
이 메소드는 문자열을 모두 대문자로 바꾼다.

### capitalize()
이 메소드는 문자열중 앞글자만 대문자로 바꾼다.

### title()
이 메소드는 문자열 중 단어 앞글자를 대문자로 만든다.

차이를 설명하자면
```python
s = 'this is a book'
print(s.upper())
print(s.capitalize())
print(s.title())
```
출력 결과
```
THIS IS A BOOK
This is a book
This Is A Book
```
조금씩 차이나는 것을 알 수 있을 것이다.
즉 title()메소드를 이용하면 문장 전체를 빈칸으로 나눠서 대문자로 치환할 수 있다.

### translate()
이 메소드도 처음 접했다.
이 메소드는 말 그대로 변환해 주는 메소드이다.

방법은 변경시켜야하는 녀석의 사전을 만들어서 인자로 넣어주면 된다.
```python
문자열.translate({ord('대상'):'변경할 내용'})
```
위와같이 사용하면 된다.
이 메소드는 여러 문자를 변환해야할 때 사용하면 매우 편리할 것이다.

위 추천 답을 보면 첫문자는 그대로 반환하고 그 뒤를 대문자화해주고 제거할 것을 제거해서 반환하도록 되어있다.

파이썬은 공부하면서 느끼지만 잘쓸 수록 코드가 쉬워지는 듯하다.
