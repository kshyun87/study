# Conditional Rendering

React에서는 필요한 동작을 캡슐화하는 고유 한 구성 요소를 만들 수 있습니다. 그런 다음 응용 프로그램의 상태에 따라 일부만 렌더링 할 수 있습니다.

React에서 조건부 렌더링(Conditional Rendering)은 JS에서 조건부와 똑같이 동작합니다.
JS에서 사용하는 if와 같은 조건부를 이용해서 현재 상태를 대표하는 엘리먼트를 생성하거나 업데이트 합니다. if/else 기억하시죠? 그대로 사용합니다.

```jsx
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```

```JSX
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```
위 두코드를 보면 두 가지 컴포넌트를 구현하고 if조건을 통해서 true면 UserGreeting엘리먼트를 false면 GuestGreeting 엘리먼트를 출력하도록 한 코드입니다.
isLoggedIn의 값에 따라서 내용이 바뀌게 되죠.

## Element Variables

```jsx
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}

class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;

    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

위코드는 로그인, 로그아웃 버튼과 관련된 내용입니다.
constructor에 기본적으로 false로 처리했기에 처음에는 false에 해당하는 내용이 렌더링됩니다. 그래서 로그인 버튼이 보이고 로그인하라고 내용이 나오는 것이죠.
버튼을 클릭하면 이벤트 헨들러가 동작하면서 state값을 true로 바꿉니다.
그럼 조건이 달라졌기때문에 로그아웃 엘리멘트가 렌더링되면서 해당 이벤트 헨들러도 같이 렌더링 되는 것이죠.

그런데 말입니다.
조건 하나는 추가하기 위해서 위와 같이 하는 것은 좋은 방법인데요. 너무 길지 않나요?
유지 보수와 개발을 위해서는 짧고 간결한 코드가 좋겠죠?
다음은 JSX의 조건을 따로 뺴서 선언하지 않고 바로 선언하는 것을 보여드리겠습니다.

## Inline If with Logical && Operator
중괄호를 이용하여 표현식을 감싸서 넣을 수 있습니다.
이것은 JS 논리 연산자 &&를 포함합니다.
엘리먼트를 조건부로 포함하는 경우에 편리할 수 있습니다.

```JSX
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```
&& 연산은 다음과 같습니다.
조건식 && 표현내용
조건식이 true면 표현내용을 표현하고
조건식이 false면 표현내용이 아닌 false로 skip하고 넘어갑니다. 즉, 아무것도 출력되지 않는 것이죠.

그럼 다른조건들을 열거하려면 어떻게 해야할까요?

## Inline If-Else with Conditional Operator
한줄에 If와 Else 쓰기

조건 ? True : False 구문으로 사용하면됩니다.
조건이 참이면 True부분을 거짓이면 False부분을 사용합니다.

```jsx
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```
위와 같이 값을 직접적으로 줄 수도 있고

```jsx
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```
이렇게 엘리먼트를 줄수도 있습니다.

어떤게 편한지는 선택입니다.
다만 조건이 복잡해 질 수도 있는데 그럴때는 컴포넌트를 분리시켜서 사용하면 좋습니다.

## Preventing Component from Rendering
다른 컴포넌트로 인해 렌더링되더라도 그 자체를 숨기고 싶은 경우가 가끔 있습니다.
 output을 렌더링하는 대신에 null을 반환하는 방법이 있습니다.


```jsx
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true};
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

ReactDOM.render(
  <Page />,
  document.getElementById('root')
);
```
WarningBanner는 warn에 따라서 렌더링 되고 안되고가 결정납니다.
이렇게 하면 조건에 따라서 출력이 되거 안되고를 만들 수 있습니다.
