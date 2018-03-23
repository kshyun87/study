React, Handling Events

이벤트 제어는 일반적인 DOM과 유사합니다.
단, 몇가지 문법적 차이가 있죠.

	* 소문자보다는 카멜표기법(camelCase)으로 이름을 작성한다.
	* 문자열보다는 함수로 전달한다.


예를 들자면
```jsx
<!-- 일반적인 HTML -->
<button onClick="button()">
    button
</button>

<!-- React -->
<button onClick={button}>
    button
</button>
```
둘의 차이가 react와 일반적인 HTML의 차이이다.

또다른 점은 false를 반환할 수 없다는 것이다. 그이유는 default 행동을 방지하기 위해서다.
그래서 preventDefault를 사용해서 명시적으로 만들어줘야한다.
그에 대한 예제는 아래에 있다.
```html
<a href="#" onClick="console.log('The link was clicked.'); return false">
       Click me
</a>
```
이렇게 작성을 하면 return을 false로 주기에 문재가 된다. 또한 이 코드는 문자열로 주기에 문제가 된다.

개발자 도구로 열어보면 에러 나도 난장판이 되어 있는 것을 볼 수 있다.
```jsx
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```
그래서 위와 같이 user-defined element로 만들어서 사용하면 에러가 없다.
물론 컴파일되면서 경고로 '#'링크 쓰지 말라고 되어 있다.

이러한 것을 합성 이벤트(synthetic event)라고 합니다.
React는 W3C스펙에 따라서 이러한 합성 이벤트를 정의합니다.
그래서 크로스 브라우징(cross-browser)에 적합한지 걱정할 필요없습니다.

크로스 브라우저(Cross-browser)?
chrome, safari, firefox, IE, Edge등 여러 종류의 브라우저에서 정상적으로 동작하는 것을 말합니다.
브라우저를 넘나들면서 정상인지를 의미하는 말입니다.


보통 React는 DOM 엘리먼트가 생성된되고 나서 이벤트를 듣기위한 listeners를 추가하기 위해 addEventListener를 호출할 필요가 없습니다. 대신에 엘리먼트가 초기 렌더링될 때 listener를 제공하면 됩니다.

ES6 클래스(class)로 컴포넌트를 정의하는 경우,
일반적인 형태는 이벤트 핸들러가 클래스의 메소드가 되도록 하는 것입니다.

토글(toggle) 컴포턴트가 사용자가 "On", Off"시키는 버튼을 렌더링합니다.
```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('test')
);
```
JSX 콜백에서의 this의 의미를 보심해야합니다.
JS(JavaScript)에서 클래스 메소드들은 기본적으로 바인딩되지 않습니다.
위 코드에서 this.nadleClick을 바인드하지 않거나 onClick에 전달하지 않는다면, this는 실제로 함수가 요청받았을 때 undefined될 것 입니다. 즉, 정의되지않아서 에러가 난다는 것입니다.

이건 React 행동 특징이 아닙니다.
JS에서 함수가 동작하는 원리와 관련되어 있는 것입니다.
일반적으로 메소드를 ()없이 사용하는 경우 그메소드는 바인드(bind) 해야합니다.

만약 bind를 호출하는 것이 귀찮다면, 2가지 방법이 있습니다.

1. 실험성의 public class fields 문법을 사용하는 경우
이 기능은 Create React App에서 일반적으로 허용하고 있습니다.
class fields방법을 사용하지 않는다면 화살표 함수를 사용해도 됩니다.
```jsx
//class fields 방법
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
//arrow function 방법
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

이런 문법의 문제는 LogginButton 렌더될때마다 다른 콜백이 생성된다는 것입니다. 대부분의 경우는 괜찮지만 이 콜백이 prop를 하위 컴포넌트로 전달하는 경우, 이런 컴포넌트들은 추가로 리렌더링(re-rendering)될 수 있습니다. 그래서 React에서는 일반적으로 이런 몇가지 문제들을 피하기 위해서 binding 또는 class fields syntax를 추천합니다.


## Passing Arguments to Event Handlers
루프 안에서 보통 추가적인 파라미터를 이벤트 헨들러로 전달하려고 합니다.

```JSX
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```
위 두 줄은 같은 내용입니다.
하나는 화살표함수(arrow function)을 사용했고 다른 하나는 바인드(bind:Function.prototype.bind)를 이용한 것이죠.
위 코드에서 e는 error가 아닌 event입니다.
두 경우 모두 React 이벤트인 e가 ID 뒤에 두번째 인자로 전달됩니다.
arrow함수인 경우 명시적으로 써야하지만, bind인경우 자동으로 추가되 코드상에서는 보이지 않습니다.
