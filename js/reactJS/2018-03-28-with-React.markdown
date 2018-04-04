---
layout: post
title: React-Usage with React
---
# Usage with React - 리엑트와 함께 쓰기
처음부터 우리는 리덕스(Redux)는 React와 아무런 관계가 없다고 강조할 필요가 있었습니다. Redux 앱을 작성할때 React도 되지만, Angular, Ember, jQeury 또는 vanila JS도 됩니다.

이말인 즉, Redux는 React나 Deku와 같은 라이브러리에서 특히 잘 동작합니다. 사용자가 UI를 함수로 설명할 수 있게 하고 Redux가 action에 대한 응답으로 state 업데이트를 내보내기 때문입니다.

먼저 react-redux를 설치하죠.
```
npm install --save react-redux
```
npm 을 이용한 설치입니다.

## 대표적인 component와 컨테이너 component
Redux의 React 바인딩은 표현형 과 컨테이너 컴포넌트로 분리하다는 아이디어를 채택했습니다. 이는 매우 중요합니다.

컴포넌트들을 좀더 쉽게 재사용할 수 있습니다. 만약 당신이 두 가지 카테고리로 분리시킨다면 말이죠. 우린 이것을 컨테이너(Container)와 Presentational(대표/표현형) 컴포넌트라고 부릅니다. 그러나 여러 다른 단어로도 불립니다. Fat&Skinny, Smart and Dumb 등등.
이런 모든 것들은 이름은 다르지만 핵심은 같습니다.

### Presentational 컴포넌트
* 어떻게 보일 것인지를 생각하는가?
* 두컴포넌트 내부를 포함하고 DOM 문법이나 스타일이 있는가?
* this.props.children을 통해 종종 차단을 허용한다.
*
||Presentational 컴포넌트|Container컴포넌트|
|-|-|-|
|목적|어떻게 보여줄것인가(markup,styles)|어떻게 동작할 것인가|
|Redux를 고려하나|아니오|예|
|데이터 읽기|props로부터 데이터 읽음.|Redux state를 subscribe함.|
|데이터 수정|porps로부터 콜백호출| 리덕스 액션을 Dispatch함|
|작성방법|수작업|보통 React Redux를 통해서 생성|

왜 이렇게 분리하는거지?
* 걱전 분리 UI와 앱을 이해하기 쉬워진다.
* 재사용성 증가.
* 표현형 컴포넌트는 기본적으로 앱의 팔레트(palette)이다. 그래서 단지 필요한 디자인을 싱글 페이지에 그릴 수 있습니다. 앱의 로직을 건들지 않고도 말이죠.
* Sidebar, Page, ContextMenu와같은 layout 컴포넌트를 추출하도록 강요합니다. 그리고 중복되는 같은 이름의 마크업이나 여러 컨테이너 컴포넌의 레이아웃을 대신해서 this.props.children을 사용합니다

우리가 작성할 대부분의 컴포넌트는 Presentational일 것입니다. 그러나 리덕스 store에 연결하기 위해서는 몇가지 Container 컴포넌트를 생성해야합니다. 만약에 컨테이너 컴포넌트가 복잡해진다면 컴포넌트 트리 안에 다른 컴테이너를 도입하는 것이 좋을 것입니다.

기술적으로 컨테이너 컴포넌트는 store.subscribe()를 사용해서 직접 작성할 수 있습니다. 그런데 React Reudx는 손으로 하기 힘든 많은 성능 최적화 작업을 만들기 때문에 추천하지는 않습니다. 
이런 이유로 컨테이너 컴포넌트를 작성하는 것보다는 react redux에서 제공하는 connect() 함수르를 이용해서 그것들을 생성합니다.

만들 앱에 UI를 생각하고 계층을 만들자.
Todo리스트 앱이라고 하면
Presentational 컴포너는트는 다음과 같이 생각할 수 있다.
일단 Todo 목록이 보여져할 것입니다.
TodoList는 todo들이 배열로 존재해야하겠죠.
Todo는 하나의 todo 아이템입니다.
그 안에는 Text와 완료여부가 필요하죠

표현한 컴포넌트를 Redux와 연결하기 위해서 컨테이너 컴포넌트를 작성해야합니다.
저장된 리스트를 불러온다거나 필터를 적용한다든가 하는 내용.
