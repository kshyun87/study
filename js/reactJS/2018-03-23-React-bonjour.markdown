---
layout: post
title:  Bonjour와 Create-react-app 사용방법.
---

# create-react-app을 이용한 경우 Bonjour 사용방법.
>일렉트론(Eelectron)과 같이 사용한 경우를 의미합니다.

이번 내용은 create-react-app을 통해서 개발환경을 구축한 경우, bonjour를 이용하려할 때 발생하는 에러를 해결하는 과정입니다.

## Bonjour(봉쥬르)?
Bonjour, 봉쥬르
우리는 프랑스의 안녕이라는 말로 기억하죠. 봉쥬르에 대한 간략한 설명을 해야할 것 같습니다.

위키 백과에는 다음과 같이 설명이 되어 있습니다.
https://ko.wikipedia.org/wiki/%EB%B4%89%EC%A3%BC%EB%A5%B4_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4)
한마디로 하면, 애플이 개발한 제로콘프(zeroconf)를 구현해주는 소프트웨어입니다. 제로콘프는 서비스 디스커버리 프로토콜입니다.
봉주르의 역할은 로컬 네트워크에 연결된 장치를 찾아내며, 그 장치들이 제공하는 서비스들을 알아내는 것 입니다.
애플이 이것을 만든 이유는 자기네 서비스를 연결하기 위한 용도로 추측됩니다.  아이튠즈를 깔면 자동으로 설치되는 요소이죠.

>참고로 제로콘프(zeroconf: zero configuration networking)는 사람이 귀찮게 네트워크 설정을 하지않아도 알아서 연결/연동되도록 하는 기법을 의미합니다.

즉 bonjour를 이용해서 네트워크에 연결된 원하는 장치를 자동으로 가져오기 위해서 사용합니다.
## node에서 bonjour 사용하기
```sh
npm install bonjour
```
위명령어로 bonjour를 설치합니다.
그리고 다음 코드를 index.js로 작성합니다.
```jsx
var bonjour = require('bonjour')()

// advertise an HTTP server on port 3000
bonjour.publish({ name: 'test_service', type: 'http', port: 5000 })
// browse for all http services
bonjour.find({ type: 'http' }, function (service) {
    console.log(service);
})
```
위 코드는 bonjour node 예제를 가지고 와서 사용한 것입니다.
publish는 서비스를 네트워크에 등록하는 것이고 find는 등록된 서비스를 찾는 것입니다.
기본적으로 위 코든 모든 서비스를 출력하기에 연결된 모든 정보를 가져옵니다.
선별해서 보고 싶은 경우는 funcion()부분에 if조건을 통해서 원하는 내용을 보면 됩니다.
sevice.name 으로 서비스명을 검색해서 보는 것도 가능합니다.

## React에서 bonjour 사용하기
이번에는 React에서 사용해보겠습니다.
index.js에 해당 내용을 추가하고 실행합니다.
```jsx
const Bonjour = require('bonjour')()
Bonjour.find({ type: 'http' }, function (service) {
 console.log('Found an HTTP server:', service)
})
```
에러가 발생합니다.
dgram.createSocket is not a function 이라는 에러가 발생되죠.
분명히 dgram은 node기본탑재로 정상적으로 node로 따로 불러서 해보면 동작됩니다.

## 왜 안되는 걸까?
일반적으로 create-react-app은 일반적으로 web-browser로 서비스한다고 가정하고 개발 환경을 구축해주는 것 같습니다.
그래서 fs(fileSystem)과 같이 PC클라이언트나 서버에서 돌아가는 녀석을 없음(empty)처리하여 webpack 환경을 만듭니다.
(최근에는 dgram도 추가된 것 같습니다.)

그런데 Electron을 이용해서 Desktop App을 만들려고 하는 경우에는 우리는 empty가 되어선 (사용안한다면 상관없지만)안됩니다.

### Webpack 설정의 문제
create-react-app을 통해서 앱을 생성하고 eject로 설정파일을 추출한 경우 conf폴더가 생성되고 그 안에 필요한 설정들이 파일로 생성됩니다.
![](assets/markdown-img-paste-20180323163500534.png)

저 목록 중에서 webpack과 관련된 것이 3가지가 있는데요. 그중 dev와 prod가 각각 개발용과 완성본의 설정파일입니다.
먼저 prod파일을 열어 봅니다.

맨 아래에 다음과 같은 내용이 있습니다.
```js
...,
  node: {
    dgram: 'empty',
    fs: 'empty',
    net: 'empty',
    tls: 'empty',
    child_process: 'empty',
  },
  ...
```
보면 node에서 사용하는 라이브러리들 중에서 다음은 empty(사용안함)처리 좀 해줘라는 의미입니다.
저걸 몰라서 계속 뭔소린가 했죠...ㅠㅠ

저 부분이 browser에서는 사용하지 않기 때문에 추가된 부분입니다.
electron인 경우에는 사용하는 부분이 생기기도 합니다. 일단 저기서 dgram을 제거 또는 주석 처리힙니다.

그리고 나서 npm run build를 수행합니다.
그래도 안됩니다.

>Module not foun: Can't resolve 'dgram' in ~

위와 같이 에러가 발생됩니다.
모듈을 찾을 수가 없다는 의미입니다.
그 이유는 webpack은 기본적으로 browser로 패키징하기에 browser에서는 fs나 dgram등이 없기에 에러가나는 것입니다.
그래서 create-react-app에서는 기본으로 empty처리한것이죠.
electron은 node.js기반이므로 fs와 같은 라이브러리를 가지고 있습니다. 그래서 electron을 통해서 dgram을 접근해야합니다.

다시inde.js 파일의 내용입니다.
```js
const Bonjour = require('bonjour')()
Bonjour.find({ type: 'http' }, function (service) {
 console.log('Found an HTTP server:', service)
})
```
위 코드에서 다음과 같이 bonjour호출을 바꿉니다.
```js
const electron = window.require('electron');
const Bonjour = electron.remote.require('bonjour')()
// const Bonjour = require('bonjour')()
Bonjour.find({ type: 'http' }, function (service) {
 console.log('Found an HTTP server:', service)
})
```
차이를 확인하기 위해서 기존 내용은 주석처리 했습니다.
window.require()를 통해서 electron을 불러오고 그 electron에서 remote를 통해 bonjour를 호출했습니다.
이렇게 하면 node에 있는 bonjour를 그대로 가져와서 사용하기에 에러가 발생되지 않습니다.
![electron-react](https://i.imgur.com/Qv3h3bb.png)
React APp을 만들고 그 밖으로 Electron으로 패키지를 만든다고 생각하면 될것 같습니다. 그래서 React App에서 Electron으로 접근하기 위해서는 내부에서 electron을 불러와야합니다. 그 방법이 위의 내용입니다.

create-react-app이 아닌 다른 개발환경이더라도 위 방법으로 하면 간단하게 불러올 수 있으니 참고하시기 바랍니다. 보통 이런 것들을 해결하기 위해서는 webpack.conf의 resolve를 수정해서 불러오는 것으로 보이는데요. 저 부분은 저렇게 하는 것이 간편하고 쉬운것 같네요. resolve는 저부분은 안될것 같기도하고 복잡하구요.

## 정리
Electron은 Main프로세스사 두가지가 있는데 하나는 Electron host/wrapper이고 다른 하나는 그 안에 개발한 App입니다. App에서 로컬 파일스시템(fs)와 같은 것에 접근하기 위해서는 다음과 같은 방법으로 접근해야합니다.
```js
const electron = window.require('electron');

const fs = electron.remote.require('fs');
```
이외의 electron에서 직접 가져오는 라이브러리들(ex: ipcRenderer)이 있는 경우, electron.xxx로 그대로 가져오셔도 됩니다.

>참고
https://medium.freecodecamp.org/building-an-electron-application-with-create-react-app-97945861647c
