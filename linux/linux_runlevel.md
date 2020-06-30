# linux runlevel 조정하기


#### 런레벨(runlevel)이란  


리눅스 시스템에는 **런레벨(runlevel)** 이라는 것이 있습니다. 일반적으로 유닉스 시스템에서 컴퓨터 OS의 작동모드 입니다.
일반적으로 0~6 까지의 7단계가 존재합니다. 어떤 특정 OS는 `S`단계가 존재하기도 합니다. 시작시의 하나의 런레벨이 실행되며
하나가 지정되어 실행되고 나면 다른 모드가 실행되지는 않습니다. 우리가 수동으로 바꿀 수 있습니다.  

런레벨은 부팅 후 리눅스 머신의 상태입니다. 우리가 일반적으로 부르는 싱글 유저모드(single-user mode), 멀티 유저모드
(multi-user mode), 시스템 종료, 시스템 리부트 등이 있습니다. 이러한 세부 설정은 리눅스 배포판에 따라서 다릅니다.
무언가 시스템 설정을 바꿔야 할 때는 싱글 유저모드에서 작업을 하기도 하고, 일반적인 GUI를 사용할 때는 디스플레이 매니저가 있는 상태인 `X 윈도우` 
환경으로 사용하기도 합니다.  

## 런레벨의 종류
런레벨의 기초가 되는 **표준 런레벨(Standard runlevels)** 이 있습니다.  

|ID  | NAME |설명    |
|:---:|:---:|:-----:|
|0|Shutdown|시스템을 종료합니다.|
|1|Single-user mode|네트워크 구성과 데몬 사용을 하지 않습니다.|
|6|Reboot|시스템을 재부팅합니다.|

우리가 터미널에서 명령어로 `init 0` 을 입력한다면 런레벨 0, 즉 시스템은 종료를 하게됩니다.
이와 마찬가지로 `init 6` 을 입력한다면 시스템은 재부팅을 하게 됩니다. 사용자가 특정 다른 값을 커널 부트 파라미터로
지정하지 않으면 시스템은 기본 실행 레벨을 시작합니다. 일반적으로 GUI환경을 사용한다면 `5` 입니다.

### Linux Standard Base specification
많은 사람들이 사용하는 우분투 OS의 경우 [LSB(Linux Standard Base)](https://en.wikipedia.org/wiki/Linux_Standard_Base) 를 준수합니다.
LSB의 시스템이 가장 기본이 되기 때문에 따로 특별한 모드를 사용하는 런레벨 시스템이 아니라면 대부분이 아래와 같이 따릅니다.

|ID  | NAME |Target|설명    |
|:---:|:---:|:---:|:-----:|
|0|Halt|poweroff.target|시스템을 종료합니다.|
|1|Single-user mode|rescue.target|관리작업 모드. 주로 시스템 설정을 바꿉니다.|
|2|Multi-user mode|multi-user.target|네트워크 서비스를 사용하지 않은 CLI 모드입니다.|
|3|Multi-user mode with networking|multi-user.target|CLI 모드에서 시스템을 정상적으로 사용합니다.|
|4|Not used/user-definable|multi-user.target|일반적으로 사용되지 않으나 유저가 정의할 수 있습니다.|
|5|Start system normally with GUI|graphical.target|런레벨 3 + GUI 모드입니다.|
|6|Reboot|reboot.target|시스템을 재부팅합니다.|


## 런레벨 변경
현재 시스템의 런레벨을 알고 싶다면 터미널에서 다음과 같이 입력합니다.
```shell script
sudo systemctl get-default
```
일반적으로 사용하는 모드는 `graphical.target`, 즉 런레벨 5 입니다. 가끔 그래픽 드라이버를 설치할 때나,
다른 그래픽 인터페이스 환경이 아닌 커맨드기반 환경에서 작업해야할 때 런레벨 3으로 변경하려합니다.
이 때는 아래와 같이 변경할 수 있습니다.
```shell script
sudo systemctl set-default multi-user.target
```
런레벨 1로 변경하고 싶다면 아래와 같이 바꿀 수 있습니다.
```shell script
sudo systemctl set-default rescue.tartget
```

런레벨 3으로 변경 이후 시스템을 재부팅하면 GUI 화면이 아닌 CLI 화면으로 전환되며 다시 GUI 모드로 변경하고 싶다면
```shell script
sudo systemctl set-default graphical.target
```
위와 같이 입력 후 재부팅을 하면 됩니다.