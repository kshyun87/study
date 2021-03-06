JavaScript 단일 페이지 응용 프로그램에 대한 요구 사항이 점차 복잡 해짐에 따라 우리 코드는 이전보다 더 많은 state를 관리해야합니다. 이 state에는 아직 서버에 저장되지 않은 로컬로 생성 된 데이터뿐만 아니라 서버 응답 및 캐시 된 데이터가 포함될 수 있습니다. 활성 state의 경로, 선택한 탭, 회 전자, 페이지 매김 제어 등을 관리해야하기 때문에 UI 상태도 복잡해지고 있습니다.

끊임없이 변화하는 state를 관리하는 것은 어렵습니다.
모델이 다른 모델을 업데이트할 수 있는 경우, 뷰도 다른 모델을 업데이트 할 수 있으며, 다른 뷰가 업데이트 될 수도 있습니다. 어떤 시점에서 당신은 그 state에 대한 통제력을 상실하기 때문에 더 이상 당신의 앱에서 무엇이 일어나고 있는지 이해하지 못하게 됩니다. 시스템이 불투명하고 결정적이지 않은 경우, 버그를 재현하거나 새로운 기능을 추가하기가 어렵습니다.

이것이 그렇게 나쁘지 않은 것처럼 프론트 엔드 제품 개발에서 새로운 요구 사항이 공통적으로 고려됩니다. 개발자는 낙관적 인 업데이트, 서버 측 렌더링, 경로 전환을 수행하기 전에 데이터 가져 오기 등을 처리해야합니다. 우리는 전에 결코 다룰 수 없었던 복잡성을 관리하려고 노력하고 있으며 필연적으로 다음과 같은 질문을합니다. 포기할건가? 내 대답은 아니오 입니다.

비동기성과 변화를 혼합시키기 어렵습니다. React와 같은 라이브러리는 비동기 및 직접 DOM조작을 제거하여 뷰 계층에서 이 문제를 해결하려고 시도하는데 데이터 상태를 관리하는 것은 당신에게 맡길 수 있습니다. 이것이 Redux가 들어가는 곳 입니다.

Redux 그 자체는 매우 단순 합니다.


state에서 무언가를 변경하기 위해서는 dispatch 해야합니다 Action이란 JS object로 무엇이 일어나는지를 설명하는 객체입니다.

강제적으로 앱안에서 무엇이 일어나는지를 분명하게 이해시킬 수 있게 모든 변화를 action으로 설명합니다.
만약 변화가 일어나면 그 변화가 왜 일어나는지를 알 수 있습니다. Action은 일어난 일의 부스러기 같은 것입니다.
마지막으로 Reducer라는 함수로 state와 action을 함께 묶습니다. 다시말하자면, 마법같은 것은 없습니다. 단지 함수입니다. state와 매개변수같은 action을 가지고 앱의 다음 state를 반환합니다. 큰 앱을 위해 함수로 작성하기는 어렵기에 작은 state의 부분을 관리하는 함수들로 작성합니다.

reducer는 State + Action 이고 함수로 되어 있습니다. 그리고 그 함수는 큰 규모의 함수가 아닌 작은 규모로 된 함수입니다.
