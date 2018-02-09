# Ubuntu MQTT(mosquitto) 설치하기
```sh
sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa
sudo apt-get update
sudo apt-get install mosquitto
sudo apt-get install mosquitto-clients
```
3번째 줄은 서버를 설치하는 명령이고
4번째 줄은 클라이언트를 설치하는 명령어

명령어를 수행하고 나면 자동적으로 서버는 구동되고 있으며
다음 명령으로 제어할 수 있다.

```
sudo service stop mosquitto
sudo service start mosquitto
```
첫번째 줄은 정지
두전째 줄은 시작
