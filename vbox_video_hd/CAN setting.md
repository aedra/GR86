# User configurable CAN setting

2023.11.19 현재 Racelogic에서 제공하는 GR86 데이터에는 유온, 수온이 빠져있다. Racelogic에서 언제 반영해줄지 알수 없으며, GR86부터 사용가능한 가속도/자이로센서 데이터도 포함시켜줄지 알수 없다.
몇몇 선구자들의 노력으로 많은 데이터의 CAN ID가 파악되었으며, https://github.com/timurrrr/ft86/blob/main/can_bus/gen2.md 를 참조하여 직접 CAN input을 구성한다.


## 설정 내역

### 공통 사항

아래 그림 참조하여 각 데이터 입력 <br>
<img src="../images/Channel settings - Coolant Temperature.png"
  alt="Location of the connector"/>
<br>

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
