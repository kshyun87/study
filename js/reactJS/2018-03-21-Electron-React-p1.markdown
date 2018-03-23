---
layout: post
title: 일렉트론과 리엑트를 같이 사용할때 생기는 문제
---
# 일렉트론과 리엑트를 같이 사용할때 생기는 문제
일렉트론과 리엑트를 같이 사용하는 경우.쉽지 않네요.

## React와 React-Redux
리엑트만 사용하는 것이 아닌 리덕스를 같이 사용하는 경우 더 복잡했습니다.

## 쉽게 react 사용하는 방법은?
create-react-app 을 설치하여 사용하는 방법이 앱을 만드는 가장 편한 방법이고 공식지원되는 라이브러리인만큼 좋습니다.
단 서버 렌더링을 사용하지 못하는 단점이 있기에 클라이언트사이드 렌더링만 할 경우에 사용하기 바랍니다.

저는 create-react-app을 이용해서 react 앱을 만들고 배포하려고 했습니다.
그런데 react-redux를 사용하려면 생성된 앱에서 eject로 풀고 src에 action, components, reducers 폴더를 각각 생성해야합니다.
기본적인 설정이 아니기 떄문입니다.

또한 필요한 경로를 추가적으로 설정하지 않으면 라이브러리가 제대로 로드되지 않거나 동작되지 않습니다.

다음 내용은 electron과 react를 사용하는 방법을 정리한 내용입니다.

## create-react-app으로 만든 react 앱을 electron으로 클라이언트 만들기

react앱을 create-react-app으로 만들면 설정값을 숨기고 나오는데 일부 변경할 이유가 있거나 하면 eject하여 변경하기 바랍니다.

먼처 작성이 완료된 앱이 있는 폴더에 main.js를 작성합니다.
```js
const {app, BrowserWindow} = require('electron')
const path = require('path');
const url = require('url');

app.on('ready', () => {


    let mainWindow = new BrowserWindow({width: 1280, height: 960,frame:false})

    console.log(process.env.DEV_URL);
    const startUrl = process.env.DEV_URL ||
    url.format({
      pathname: path.join(__dirname, '/build/index.html'),
      protocol: 'file:',
      slashes: true
    });
    console.log(startUrl);
    mainWindow.loadURL(startUrl);
})
```
이러면 1단계 끝입니다.

그다음 pakages.json 파일을 수정해야합니다.
```json
"homepage": "./",
"main": "main.js",
"scripts": {
    "start": "node scripts/start.js",
    "build": "node scripts/build.js",
    "test": "node scripts/test.js --env=jsdom",
    "electron": "electron ."
  },
```
homepage와 main을 추가해주고 scripts에 electron부분을 추가해줍니다.
hompage는 경로때문에 필요한 부분입니다.
main은 electron을 사용하기 위한 스크립트입니다.
나중에 scripts는 npm을 통해 명령을 수행할 수 있는 스크립트 입니다.

```sh
npm run electron
```
위 명령어로 실행하면 실행됩니다.
단, 해당 내용은 build를 한 후에 적용되니 변경한 내용이 있다면 build한번 해주고 실행해주시기 바랍니다.
자동 로드되는건 다음에 추가할 예정입니다.(있는듯 해서요)

## react-window-titlebar를 사용할때 주의사항
혹시 react-window-titlebar 라이브러리를 사용하는 사람이 있는 경우에 문제 해결방법을 적습니다.
```jsx
import TitleBar from 'react-window-titlebar'; // import TitleBar // 바가 생김.
import {remote} from 'electron'; // get remote object from electron which helps Titlebar to control window actions
'''
render(){
    return(
        <TitleBar title="Title" remote={remote}  theme="dark" />
    )
}

'''
```
위와 같이 라이브러리 예제에 있는데요 create-react-app을 사용하면 fs에러가 발생됩니다. electron의 remote를 제대로 불러오지 못해서 에러가 발생됩니다.

그래서 찾은 방법은 불러오기 방법을 조금 바꾸면 됩니다.
```jsx
const {remote} = window.require('electron')
```
이렇게 하면 제대로 동작합니다.
electron이 실행되고 나서 그 안에서 참조하도록 하는 것으로 보입니다.
window.require()는 열려 있는 창에서 라이브러리를 불러오는 방법으로 electron이 동작되고 있기 떄문에 electron으로 열린 화면에 라이브러리를 추가하는 것입니다.
이 방법은 browser에서는 동작하지 않습니다.
browser에서 window.require()가 정의되어 있지 않습니다.
