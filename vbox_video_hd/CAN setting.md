# User configurable CAN setting

2023.11.19 현재 Racelogic에서 제공하는 GR86 데이터에는 유온, 수온이 빠져있다. Racelogic에서 언제 반영해줄지 알수 없으며, GR86부터 사용가능한 가속도/자이로센서 데이터도 포함시켜줄지 알수 없다.
몇몇 선구자들의 노력으로 많은 데이터의 CAN ID가 파악되었으며, https://github.com/timurrrr/ft86/blob/main/can_bus/gen2.md 를 참조하여 직접 CAN input을 구성한다.


## 설정 내역

### VBOX Video 설명

아래 그림은 VBOX Video 소프트웨어에서 CAN input을 직접 설정하는 메뉴이다. <br>
<img src="../images/Channel settings - Coolant Temperature.png"
  alt="Location of the connector"/>
<br>

각 필드에 대한 설명은 다음과 같다.

Name | 설명
---- | ----
Name | Scene 파일이나 Math에서 사용할 이름
Units | 단위
Scale | CAN으로 추출한 데이터 x를 바탕으로 원하는 값 y = a x + b로 계산할 때 기울기 a에 해당하는 값
Offset | 위 식 y = a x + b 에서 b에 해당하는 값
---- | ----
Signal type | 그냥 Signal로 설정
Id (hex) | CAN ID. 16진수 값으로 입력
Is extended | Extended frame일 경우 선택. 일단 비워둘것
Start bit | Dataframe 중 몇번째 비트부터 원하는 데이터가 있는지 지정
Length | 데이터 길이. Bit 단위로 입력
DLC | Data Length Code. 해당 패킷에서 데이터가 몇바이트 들어가 있는지 지정. 8 바이트 선택
Maximum | Scale, offset 적용할 경우 최대 최소값 표시됨. 설정 시에는 그냥 0으로 입력해도 됨
Minimum | 위와 동일
Byte order | Little endian이면 Intel 선택. 아니면 Motorola 선택
Data format | 부호 없는 정수일 경우 Unsigned 선택. 부호 있는 정수이면 Signed 선택. 나머지는 부동소수점일 경우 선택하는건데 이건 대부분 사용하지 않는것 같음




### 공통 사항


Signal type : Signal <br>
DLC : 8 <br>
Byte order : Intel <br>
Maximum과 minimum은 0으로 두고 저장하면 자동으로 반영되는 것으로 보임 <br>

### 신호별 설정



Name | Units | Scale | Offset | ID(HEX) | Start bit | Length | Data format 
---- | ----- | ----- | ------ | ------- | --------- | ------ | -----------
Oil temperature | °C | 1.0 | -40.0 | 345 | 24 | 8 | Unsigned
Coolant temperature | °C | 1.0 | -40.0 | 345 | 32 | 8 | Unsigned
Lateral acceleration | g | 0.02041 | 0.0 | 13B | 48 | 8 | Signed
Longitudinal acceleration | g | -0.01020 | 0.0 | 13B | 56 | 8 | Signed
Yaw rate | ? | -0.27250 | 0.0 | 138 | 32 | 16 | Signed

Lateral/longitudinal acceleration은 원본 scale을 사용할 경우 m/sec<sup>2</sup>로 계산됨. 위 식에서는 9.8로 나눠서 g로 표시.<br>
Yaw rate 단위가 불분명한데, °/sec로 추정됨. 추후 확인 필요함. 그리고 ID가 백삼십팔임. Acceleration의 13B와 헛갈리지 말것. <br>

### Scene 파일

GR86 SWLEE 20231118.REF : 위 신호별 설정과 2023.11.19 현재 Racelogic에서 제공하는 데이터를 추가하여 작성한 사용자 데이터베이스 파일

GR86 SCN1 R1.VVHSN : 아래 그림 형태의 scene 파일

<img src="../images/Screen capture - GR86 SCN1 R1.PNG"
  alt="Location of the connector"/>
