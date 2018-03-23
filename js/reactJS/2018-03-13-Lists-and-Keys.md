# Lists and Keys

리스트를 사용하려면 어떻게 해야할까요?
map()함수를 이용하면 리스트를 가지고 각기 같은 동작을 수행할 수 있습니다.

```jsx
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```
 위 numbers 객체에 있는 리스트를 map함수로 각각의 값의 2배씩 해서 doubled로 지정했다.
 콘솔창에 doubled값을 확인하면 두배씩 커진 값을 확인할 수 있습니다.


 React에서는 배열을 엘리먼트 목록으로 거의 동일하게 바꿀 수 있습니다.


 ## Rendering Multiple Components

엘리먼트를 포함하여 만들 수도 있다.
이때 값은 {} 중괄호를 이용하여 표현합니다.


```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number)=>
    <li>{number}</li>
);
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```


위 코드를 보면 numbers라는 리스트를 map함수로 각각 "< li >" 엘리먼트로 바꿨습니다.
그리고 render에서 < ul >엘리먼트 안에 listItems가 사용되어 변환된 내용을 렌더링 하더록 했습니다.
다음과 같이 출력됩니다.

![](assets/markdown-img-paste-20180313182113177.png)

## Basic List Component

일반적으로 컴포넌트 안에 리스트를 렌더링하게 됩니다.
