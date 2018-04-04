
# react-split-pane
페이지를 만들때 layout을 구분지어서 만들거나 나누고 싶을때가 있습니다.

대시보드 같은 경우 좌측은 메뉴로 나머지는 뷰 페이지로 만드는 경우가 많고
일반적인 홈페이지는 상단은 header로 메뉴로 분류하고 나머지를 뷰 페이지로 만들어진 것을 볼 수 있습니다.

react-split-pane은 이런 layout을 나눠서 구성해주는 컴포넌트입니다.
단순히 새로분할, 가로 분할 두가지 기능이 있는데요.
중첩으로 사용하면 복잡한 layout을 쉽게 만들 수 있습니다.


```sh
npm install --save react-split-pane
```
>node와 react가 설치 되어 있다고 보고 진행합니다.

```jsx
import SplitPane from 'react-split-pane/lib/SplitPane';

const splitView = ()=> {
    <SplitPane split="horizontal" minSize={30} defaultSize={30} style={{height:30}} >
    <div>layout 1</div>
    <div>layout 2</div>
    </SplitPane>
}
```
layout 1과 layout 2가 가로로 분할 되어 나타나게 될 것입니다.

응용해서 div 테그 안에 다시 SplitPane을 사용하면 또 그 화면을 분할할 수 있습니다.
