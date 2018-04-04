#React-Redux

웹에서 발생된 행위(action)은 reducer로 전달되어 store에 있는 state를 변경할 수 있다. 변경된 state에 따라 다시 view가 업데이트 된다.
action을 store로 연결하는 것을 dispatch라고 하며

store에 있는 state를 view로 가져오려면 connect를 이용해서 react의 props로 Redux의 state를 가져올 수 있다.
