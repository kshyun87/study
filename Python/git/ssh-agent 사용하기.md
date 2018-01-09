# ubuntu에서 git 사용하기

ssh-agent 가 제대로 동작하는지 확인하기

```bash
eval $(ssh-agent -s)
```
이렇게 동작시키면 ssh-agent가 동작하고 있고

다음과 같이 키를 추가해주면 된다.
```bash
ssh-add ~/.ssh/xxx
```
그러나 이때
```
Could not open a connection to your authentication agent.
```

요런 문구가 나오면서 동작하지 않을 때가 있다.
그건 바로 인증키에 대한 권한 문제가 있어서다.
/.ssh 폴더를 사용할 때 agent가 sudo권한이 아닌 상태로 동작하고 있고
sudo ssh-add를 사용해서 권한으로 추가하려다 보니 최상위 권한으로 동작되고 있는 agent가 없다.
그래서 .ssh를 폴더의 권한을 변경하고 현재 사용자에게 지정해줘야한다.
```bash
sudo chown -R username:username ~/.ssh
chown 700 ~/.ssh
```
이렇게 해주면 정상적으로 동작한다.
