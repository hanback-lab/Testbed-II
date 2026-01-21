# Pop API 
Pop 은 한백전자의 장비를 제어하는 파이썬 라이브러리입니다. pip를 통해 다운로드 받아 설치가 가능합니다. 

## pop-testbed 설치 
```sh
pip install pop-testbed 
```

### 연결 설정 파일 생성 
pop-testbed 활용에 앞서 연결 설정 파일을 생성합니다. 접속할 브로커의 주소, 장비 고유번호, 등의 정보를 "product" 파일로 저장하고 해당 정보를 읽어 활용합니다. 아래는 작성 예시입니다.

```conf
BROKER_DOMAIN=127.0.0.1
CAMERA_DOMAIN=127.0.0.1
DEVICE_NAME=TB
DEV_NUM=01
INSITUTION_NAME=HBE
```

- BROKER_DOMAIN : 접속할 브로커의 주소를 입력합니다. TestBed의 HMI IP를 입력합니다.
- DEVICE_NAME : 장비의 이름으로 TestBed 은 'TB' 으로 기본 설정되어 있습니다. 
- DEV_NUM : 장치의 고유 번호로 여러개의 장비가 존재하는 경우에는 이 번호를 중복되지 않게 설정해야 합니다. 
- INSITUTION_NAME : 학교 또는 기관의 명칭을 고유 키워드로 활용합니다. 

product 파일은 pop-testbed 을 활용하여 작성된 파이썬 프로그램을 실행하는 위치에 존재해야합니다. 

## testbed.actuator 
TestBed 의 액츄에이터를 제어하는 API가 포함되어 있습니다. 기본적인 활용은 다음과 같습니다. 

```python
from testbed.actuator import Lamp 
```

testbed.actuator 에 포함된 기능은 다음과 같습니다. 

---
### Class Lamp 
단일 Lamp 제어 클래스 
- Lamp.PLACE : Lamp 가 TestBed에 설치된 장소, 'ENTRANCE' or 'ROOM' or 'KITCHEN'
- Lamp.NAME : 장치 명칭
- Lamp.on() : Lamp 켜기
- Lamp.off() : Lamp 끄기
- Lamp.state : Lamp 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ts':1609460954, 'state':'ON'}
---
### Class LampGroup
전체 Lamp 제어 클래스 
- Lamp.on(place=None) : 전체 Lamp 켜기, place 에 장소 지정하면 해당 구역만 켜기 
    - place = 'ENTRANCE' or 'ROOM' or 'KITCHEN' 
- Lamp.off(place=None) : 전체 Lamp 끄기, place 에 장소 지정하면 해당 구역만 끄기 
    - place = 'ENTRANCE' or 'ROOM' or 'KITCHEN' 
- Lamp.state : Lamp 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ENTRANCE': None, 'ROOM': {'ts': 1609467214, 'state': 'ON'}, 'KITCHEN': {'ts': 1609467208, 'state': 'OFF'}}
---

### Class Fan 
단일 Fan 제어 클래스 
- Fan.PLACE : Fan 이 TestBed에 설치된 장소, 'ROOM' or 'KITCHEN'
- Fan.NAME : 장치 명칭
- Fan.on() : Fan 켜기
- Fan.off() : Fan 끄기
- Fan.state : Fan 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ts':1609460958, 'state':'OFF'}

### Class FanGroup 
전체 Fan 제어 클래스 
- Fan.on(place=None) : Fan 켜기, place 에 장소 지정하면 해당 구역만 켜기 
    - place = 'ROOM' or 'KITCHEN' 
- Fan.off(place=None) : Fan 끄기, place 에 장소 지정하면 해당 구역만 끄기 
    - place = 'ROOM' or 'KITCHEN' 
- Fan.state : Fan 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ENTRANCE': {'ts': 1609467209, 'state': 'OFF'}, 'ROOM': {'ts': 1609467214, 'state': 'ON'}, 'KITCHEN': {'ts': 1609467208, 'state': 'OFF'}}

### Class DoorLock 
DoorLock 제어 클래스 
- DoorLock.PLACE : DoorLock 이 TestBed에 설치된 장소, 'ENTRNACE'
- DoorLock.NAME : 장치 명칭
- DoorLock.open() : DoorLock 열기 
- DoorLock.close() : DoorLock 닫기
- DoorLock.state : DoorLock 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ts':1609461058, 'state':'OPEN'}

### Class GasBreaker 
GasBreaker 제어 클래스 
- GasBreaker.PLACE : GasBreaker 이 TestBed에 설치된 장소, 'KITCHEN'
- GasBreaker.NAME : 장치 명칭
- GasBreaker.open() : GasBreaker 열기 
- GasBreaker.close() : GasBreaker 닫기
- GasBreaker.state : GasBreaker 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ts':1609461059, 'state':'OPEN'}

### Class Curtain 
Curtain 제어 클래스 
- Curtain.PLACE : Curtain 이 TestBed에 설치된 장소, 'ROOM'
- Curtain.NAME : 장치 명칭
- Curtain.open() : Curtain 열기 
- Curtain.close() : Curtain 닫기
- Curtain.stop() : Curtain 제어 중지 
- Curtain.state : Curtain 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ts':1609462059, 'state':'OPENED'}

## testbed.sensor

### Class Pir 
Pir 센서 클래스 
- Pir.PLACE : Pir 이 TestBed에 설치된 장소, 'ENTRANCE'
- Pir.NAME : 장치 명칭
- Pir.read() : Pir 센서값 읽기
    - ex) {'ts': 1609478022, 'state': 0}

### Class Dust 
Dust 센서 클래스 
- Dust.PLACE : Dust 이 TestBed에 설치된 장소, 'ROOM'
- Dust.NAME : 장치 명칭
- Dust.read() : Dust 센서값 읽기, tsi 방식 
    - ex) {'ts': 1609478030, 'pm1_0': 12, 'pm2_5': 16, 'pm10': 18} 

### Class Tphg 
단일 TPHG(Temperature, Pressure, Humidity, Gas) 센서 클래스 
- Tphg.PLACE : Tphg 이 TestBed에 설치된 장소, 'ENTRANCE', 'ROOM', 'KITCHEN'
- Tphg.NAME : 장치 명칭
- Tphg.read() : Tphg 센서값 읽기
    - ex) {'ts': 1609478223, 'temp': 19.57, 'humi': 12.64, 'press': 1010.86, 'gas': 12946}

### Class TphgGroup 
전체 TPHG(Temperature, Pressure, Humidity, Gas) 센서 클래스 
- Tphg.read(places=None) : Tphg 센서값 읽기, places 에 장소 지정하면 해당 구역만 켜기 
    - ex) {'ENTRANCE': {'ts': 1609478310, 'temp': 19.53, 'humi': 12.59, 'press': 1010.78, 'gas': 12946}, 'ROOM': {'ts': 1609480560, 'temp': 21.33, 'humi': 9.57, 'press': 1011.55, 'gas': 12887}, 'KITCHEN': {'ts': 1609470309, 'temp': 21.68, 'humi': 9.83, 'press': 1011.29, 'gas': 12946}}

### Class Light 
단일 Light 센서 클래스 
- Light.PLACE : Light 이 TestBed에 설치된 장소, 'ENTRANCE', 'ROOM', 'KITCHEN'
- Light.NAME : 장치 명칭
- Light.read() : Light 센서값 읽기
    - ex) {'ts': 1609475696, 'state': 264}

### Class LightGroup 
전체 Lihgt 센서 클래스 
- Lihgt.read(places=None) : Light 센서값 읽기, places 에 장소 지정하면 해당 구역만 켜기 
    - ex) {'ENTRANCE': {'ts': 1609483788, 'state': 194}, 'ROOM': {'ts': 1609486038, 'state': 198}, 'KITCHEN': {'ts': 1609475785, 'state': 262}}

### Class GasDetector 
GasDetector 클래스 
- GasDetector.PLACE : GasDetector 이 TestBed에 설치된 장소, 'KITCHEN'
- GasDetector.NAME : 장치 명칭
- GasDetector.read() : Gas 센서값 읽기
    - ex) {'ts': 1609476041, 'state': 0}

### Class Impact 
개별 Impact 클래스 
- Impact.PLACE : Impact 가 TestBed에 설치된 장소, 'ENTRANCE', 'ROOM', 'KITCHEN'
- Impact.NAME : 장치 명칭
- Impact.read() : Accel 센서값 읽기
    - ex) {'ts': 1609476426, 'state': 'none'}

### Class ImpactGroup 
전체 Impact 센서 클래스 
- Impact.read(places=None) : Light 센서값 읽기, places 에 장소 지정하면 해당 구역만 켜기 
    - ex) {'ENTRANCE': None, 'ROOM': {'ts': 1609476471, 'state': 'none'}, 'KITCHEN': {'ts': 1609476471, 'state': 'detect'}}
