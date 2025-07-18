# Pop API 
Pop 은 한백전자의 장비를 제어하는 파이썬 라이브러리입니다. pip를 통해 다운로드 받아 설치가 가능합니다. 

## pop-xhome 설치 
```sh
pip install pop-xhome 
```

### 연결 설정 파일 생성 
pop-xhome 활용에 앞서 연결 설정 파일을 생성합니다. 접속할 브로커의 주소, 장비 고유번호, 등의 정보를 "product" 파일로 저장하고 해당 정보를 읽어 활용합니다. 아래는 작성 예시입니다.

```conf
BROKER_DOMAIN=127.0.0.1
DEVICE_NAME=xhome
DEV_NUM=01
INSITUTION_NAME=hbe
```

BROKER_DOMAIN 에는 접속할 브로커의 주소를 입력합니다. 기본은 "127.0.0.1" 입니다. DEVICE_NAME 은 장비의 이름으로 XHome 은 'xhome' 으로 기본 설정되어 있습니다. DEV_NUM 은 장치의 고유 번호로 여러개의 장비가 존재하는 경우에는 이 번호를 중복되지 않게 설정해야 합니다. INSITUTION_NAME 은 학교 또는 기관의 명칭을 고유 키워드로 활용합니다. 

product 파일은 pop-xhome 을 활용하여 작성된 파이썬 프로그램을 실행하는 위치에 존재해야합니다. 

## xhome.actuator 
XHome 의 액츄에이터를 제어하는 API가 포함되어 있습니다. 기본적인 활용은 다음과 같습니다. 

```python
from xhome.actuator import Lamp 
```

xhome.actuator 에 포함된 기능은 다음과 같습니다. 

### Class Lamp 
 
- Lamp.PLACE : Lamp 가 XHome에 설치된 장소, 'entrance','livingroom','kitchen','room','bathroom'
- Lamp.NAME : 장치 명칭
- Lamp.on(target=None) : Lamp 켜기, target=None 인 경우 전체 제어 
    - target : 장소 명칭 
- Lamp.off(target=None) : Lamp 끄기, target=None 인 경우 전체 제어 
    - target : 장소 명칭 
- Lamp.state : Lamp 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'entrance' : 'on' ,'livingroom' : 'off' ,'kitchen' : 'None' ,'room' : 'off' ,'bathroom' : 'on' }

### Class Fan 

- Fan.PLACE : Fan 이 XHome에 설치된 장소, 'livingroom','kitchen','room','bathroom'
- Fan.NAME : 장치 명칭
- Fan.on(target=None) : Fan 켜기, target=None 인 경우 전체 제어 
    - target : 장소 명칭 
- Fan.off(target=None) : Fan 끄기, target=None 인 경우 전체 제어 
    - target : 장소 명칭 
- Fan.state : Fan 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'livingroom' : 'off' ,'kitchen' : 'None' ,'room' : 'None' ,'bathroom' : 'on' }

### Class DoorLock 

- DoorLock.PLACE : DoorLock 이 XHome에 설치된 장소, 'entrance'
- DoorLock.NAME : 장치 명칭
- DoorLock.open() : DoorLock 열기 
- DoorLock.close() : DoorLock 닫기
- DoorLock.state : DoorLock 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'entrance' : 'open'}

### Class GasBreaker 

- GasBreaker.PLACE : GasBreaker 이 XHome에 설치된 장소, 'kitchen'
- GasBreaker.NAME : 장치 명칭
- GasBreaker.open() : GasBreaker 열기 
- GasBreaker.close() : GasBreaker 닫기
- GasBreaker.state : GasBreaker 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'kitchen' : 'open'}

### Class Curtain 

- Curtain.PLACE : Curtain 이 XHome에 설치된 장소, 'room'
- Curtain.NAME : 장치 명칭
- Curtain.open() : Curtain 열기 
- Curtain.close() : Curtain 닫기
- Curtain.stop() : Curtain 제어 중지 
- Curtain.state : Curtain 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'room' : 'close'}

### Class MoodLamp 

- MoodLamp.PLACE : MoodLamp 이 XHome에 설치된 장소, 'home'
- MoodLamp.NAME : 장치 명칭
- MoodLamp.setColor(r=255,g=255,b=255) : MoodLamp 색상 설정 
    - r : RED 색상, 0 ~ 255
    - g : GREEN 색상, 0 ~ 255
    - b : BLUE 색상, 0 ~ 255
- MoodLamp.off() : MoodLamp 색상 설정 해제 

## xhome.sensors

### Class Pir 

- Pir.PLACE : Pir 이 XHome에 설치된 장소, 'entrance'
- Pir.NAME : 장치 명칭
- Pir.read() : Pir 센서값 읽기
    - 'detect' or 'not detect' 
- Pir.callback(func,repeat=1000,param=None) : 콜백 등록 
    - func : 콜백 메소드 
    - repeat : 반복 주기, 1000은 1초 
    - param : 콜백 메소드 전달 인자 
- Pir.stop() : 콜백 해제  

### Class Dust 

- Dust.PLACE : Dust 이 XHome에 설치된 장소, 'livingroom'
- Dust.NAME : 장치 명칭
- Dust.read() : Dust 센서값 읽기, tsi 방식 
    - ex) {'1.0': 18, '2.5': 19, '10': 19} 
- Dust.callback(func,repeat=1000,param=None) : 콜백 등록 
    - func : 콜백 메소드 
    - repeat : 반복 주기, 1000은 1초 
    - param : 콜백 메소드 전달 인자 
- Dust.stop() : 콜백 해제  

### Class Tphg 

- Tphg.PLACE : Tphg 이 XHome에 설치된 장소, 'livingroom'
- Tphg.NAME : 장치 명칭
- Tphg.read() : Tphg 센서값 읽기
    - ex) {'temperature': 24, 'humidity': 38, 'pressure': 982, 'gas': 12917}
- Tphg.callback(func,repeat=1000,param=None) : 콜백 등록 
    - func : 콜백 메소드 
    - repeat : 반복 주기, 1000은 1초 
    - param : 콜백 메소드 전달 인자 
- Tphg.stop() : 콜백 해제  

### Class Gas 

- Gas.PLACE : Gas 이 XHome에 설치된 장소, 'kitchen'
- Gas.NAME : 장치 명칭
- Gas.read() : Gas 센서값 읽기
    - ex) 33
- Gas.callback(func,repeat=1000,param=None) : 콜백 등록 
    - func : 콜백 메소드 
    - repeat : 반복 주기, 1000은 1초 
    - param : 콜백 메소드 전달 인자 
- Gas.stop() : 콜백 해제  

### Class Light 

- Light.PLACE : Light 이 XHome에 설치된 장소, 'room'
- Light.NAME : 장치 명칭
- Light.read() : Light 센서값 읽기
    - ex) 414
- Light.callback(func,repeat=1000,param=None) : 콜백 등록 
    - func : 콜백 메소드 
    - repeat : 반복 주기, 1000은 1초 
    - param : 콜백 메소드 전달 인자 
- Light.stop() : 콜백 해제  

### Class Reed 

- Reed.PLACE : Reed 이 XHome에 설치된 장소, 'room'
- Reed.NAME : 장치 명칭
- Reed.read() : Reed 센서값 읽기
    - ex) "open"
- Reed.callback(func,repeat=1000,param=None) : 콜백 등록 
    - func : 콜백 메소드 
    - repeat : 반복 주기, 1000은 1초 
    - param : 콜백 메소드 전달 인자 
- Reed.stop() : 콜백 해제  

### Class Accel 

- Accel.PLACE : Accel 이 XHome에 설치된 장소, 'home'
- Accel.NAME : 장치 명칭
- Accel.read() : Accel 센서값 읽기
    - ex) {'x': 0.016, 'y': -0.48, 'z': -0.888}
- Accel.callback(func,repeat=1000,param=None) : 콜백 등록 
    - func : 콜백 메소드 
    - repeat : 반복 주기, 1000은 1초 
    - param : 콜백 메소드 전달 인자 
- Accel.stop() : 콜백 해제  