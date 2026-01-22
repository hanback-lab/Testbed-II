# Pop API 
Pop 은 한백전자의 장비를 제어하는 파이썬 라이브러리입니다. pip를 통해 다운로드 받아 설치가 가능합니다. 

## pop-testbed 설치 
```sh
pip install pop-testbed 
```

### 연결 설정 파일 생성 
pop-testbed 활용에 앞서 연결 설정 파일을 생성합니다. 접속할 브로커의 주소, 카메라 서버 주소, 장비 고유번호 등의 정보를 "product" 파일로 생성하여 **자신의 작업공간**에 저장하고 해당 정보를 읽어 활용합니다. 

#### EdgeAI product 파일 내용 확인
product 파일 생성에 앞서 TestBed 에 작성되어 있는 내용을 확인해 보겠습니다. 

TestBed와의 네트워크 연결은 `Chapter1. TestBed II -> 1. TestBed II 개요 -> TestBed 네트워크 연결` 부분을 참고합니다. 

네트워크 연결을 먼저 진행한 후 아래 내용을 진행합니다. 

1. 터미널 실행   
    - `Win + x`를 눌러 표시되는 작업 표시줄 메뉴에서 '터미널' 을 실행합니다. 

2. SSH 접속  
    - 터미널 창에서 아래와 같이 명령어를 입력합니다.

```sh 
ssh soda@192.168.50.2
```

3. 다음 명령을 통한 명칭 확인   
    - 다음 명령을 입력하여 출력되는 내용중 `INSITUTION_NAME` 을 확인하여 작성중인 product 파일에 적용합니다. 

```sh 
cat /etc/product 

ex) 
BROKER_DOMAIN=127.0.0.1
CAMERA_DOMAIN=127.0.0.1
DEVICE_NAME=TB
DEV_NUM=01
INSITUTION_NAME=HBE
```

출력되는 product 파일의 내용은 TestBed 내부에 저장된 내용이며 출력된 내용의 정보는 다음과 같습니다. 

- BROKER_DOMAIN : 접속할 브로커의 주소 입니다. 
    - TestBed 의 EdgeAI 는 자체 설치된 브로커를 활용합니다. 따라서 IP 주소가 '127.0.0.1' 로 설정되어 있습니다. 
    - **외부에서 접속하는경우에는 EdgeAI 의 IP인 '192.168.50.2' 를 입력합니다.**
- CAMERA_DOMAIN : TestBed 의 Camera 서버 주소 입니다. 
    - **외부에서 접속하는경우에는 EdgeAI 의 IP인 '192.168.50.2' 를 입력합니다.**
- DEVICE_NAME : 장비의 이름으로 TestBed 은 'TB' 으로 설정되어 있습니다. 
- DEV_NUM : 장치의 고유 번호로 여러개의 장비가 존재하는 경우에는 이 번호를 중복되지 않게 설정해야 합니다. 
- INSITUTION_NAME : 학교 또는 기관의 명칭을 고유 키워드로 활용합니다. 

이 정보를 기반으로 product 파일을 **자신의 PC의 작업공간**에 파일을 생성합니다. 

파일생성에 앞서 작업공간을 지정합니다. VSCode 에서는 `File -> Open Folder` 선택 후 원하는 폴더를 지정합니다. 작업공간을 지정했다면 다음과 같이 **PC 에서 접속하기위한 product 파일** 을 작성합니다. 

```conf
BROKER_DOMAIN=192.168.50.2
CAMERA_DOMAIN=192.168.50.2
DEVICE_NAME=TB
DEV_NUM=01
INSITUTION_NAME=HBE
```

product 파일 생성 후 작업공간에 다음 그림과 같이 파일이 존재하는것을 확인 후 프로그램 실습을 진행합니다. 각각의 명칭과 정보를 정확하게 기입해야 이후 제어 프로그램 실행하는 과정에서 정상적인 제어를 진행할 수 있습니다.

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
---

### Class FanGroup 
전체 Fan 제어 클래스 
- Fan.on(place=None) : Fan 켜기, place 에 장소 지정하면 해당 구역만 켜기 
    - place = 'ROOM' or 'KITCHEN' 
- Fan.off(place=None) : Fan 끄기, place 에 장소 지정하면 해당 구역만 끄기 
    - place = 'ROOM' or 'KITCHEN' 
- Fan.state : Fan 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ENTRANCE': {'ts': 1609467209, 'state': 'OFF'}, 'ROOM': {'ts': 1609467214, 'state': 'ON'}, 'KITCHEN': {'ts': 1609467208, 'state': 'OFF'}}
---

### Class DoorLock 
DoorLock 제어 클래스 
- DoorLock.PLACE : DoorLock 이 TestBed에 설치된 장소, 'ENTRNACE'
- DoorLock.NAME : 장치 명칭
- DoorLock.open() : DoorLock 열기 
- DoorLock.close() : DoorLock 닫기
- DoorLock.state : DoorLock 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ts':1609461058, 'state':'OPEN'}
---

### Class GasBreaker 
GasBreaker 제어 클래스 
- GasBreaker.PLACE : GasBreaker 이 TestBed에 설치된 장소, 'KITCHEN'
- GasBreaker.NAME : 장치 명칭
- GasBreaker.open() : GasBreaker 열기 
- GasBreaker.close() : GasBreaker 닫기
- GasBreaker.state : GasBreaker 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ts':1609461059, 'state':'OPEN'}
---

### Class Curtain 
Curtain 제어 클래스 
- Curtain.PLACE : Curtain 이 TestBed에 설치된 장소, 'ROOM'
- Curtain.NAME : 장치 명칭
- Curtain.open() : Curtain 열기 
- Curtain.close() : Curtain 닫기
- Curtain.stop() : Curtain 제어 중지 
- Curtain.state : Curtain 의 제어 피드백, 제어가 없는 경우에는 None 
    - ex) {'ts':1609462059, 'state':'OPENED'}
---

## testbed.sensor
---

### Class Pir 
Pir 센서 클래스 
- Pir.PLACE : Pir 이 TestBed에 설치된 장소, 'ENTRANCE'
- Pir.NAME : 장치 명칭
- Pir.read() : Pir 센서값 읽기
    - ex) {'ts': 1609478022, 'state': 0}
---

### Class Dust 
Dust 센서 클래스 
- Dust.PLACE : Dust 이 TestBed에 설치된 장소, 'ROOM'
- Dust.NAME : 장치 명칭
- Dust.read() : Dust 센서값 읽기, tsi 방식 
    - ex) {'ts': 1609478030, 'pm1_0': 12, 'pm2_5': 16, 'pm10': 18} 
---

### Class Tphg 
단일 TPHG(Temperature, Pressure, Humidity, Gas) 센서 클래스 
- Tphg.PLACE : Tphg 이 TestBed에 설치된 장소, 'ENTRANCE', 'ROOM', 'KITCHEN'
- Tphg.NAME : 장치 명칭
- Tphg.read() : Tphg 센서값 읽기
    - ex) {'ts': 1609478223, 'temp': 19.57, 'humi': 12.64, 'press': 1010.86, 'gas': 12946}
---

### Class TphgGroup 
전체 TPHG(Temperature, Pressure, Humidity, Gas) 센서 클래스 
- Tphg.read(places=None) : Tphg 센서값 읽기, places 에 장소 지정하면 해당 구역만 켜기 
    - ex) {'ENTRANCE': {'ts': 1609478310, 'temp': 19.53, 'humi': 12.59, 'press': 1010.78, 'gas': 12946}, 'ROOM': {'ts': 1609480560, 'temp': 21.33, 'humi': 9.57, 'press': 1011.55, 'gas': 12887}, 'KITCHEN': {'ts': 1609470309, 'temp': 21.68, 'humi': 9.83, 'press': 1011.29, 'gas': 12946}}
---

### Class Light 
단일 Light 센서 클래스 
- Light.PLACE : Light 이 TestBed에 설치된 장소, 'ENTRANCE', 'ROOM', 'KITCHEN'
- Light.NAME : 장치 명칭
- Light.read() : Light 센서값 읽기
    - ex) {'ts': 1609475696, 'state': 264}
---

### Class LightGroup 
전체 Lihgt 센서 클래스 
- Lihgt.read(places=None) : Light 센서값 읽기, places 에 장소 지정하면 해당 구역만 켜기 
    - ex) {'ENTRANCE': {'ts': 1609483788, 'state': 194}, 'ROOM': {'ts': 1609486038, 'state': 198}, 'KITCHEN': {'ts': 1609475785, 'state': 262}}
---

### Class GasDetector 
GasDetector 클래스 
- GasDetector.PLACE : GasDetector 이 TestBed에 설치된 장소, 'KITCHEN'
- GasDetector.NAME : 장치 명칭
- GasDetector.read() : Gas 센서값 읽기
    - ex) {'ts': 1609476041, 'state': 0}
---

### Class Impact 
개별 Impact 클래스 
- Impact.PLACE : Impact 가 TestBed에 설치된 장소, 'ENTRANCE', 'ROOM', 'KITCHEN'
- Impact.NAME : 장치 명칭
- Impact.read() : Accel 센서값 읽기
    - ex) {'ts': 1609476426, 'state': 'none'}
---

### Class ImpactGroup 
전체 Impact 센서 클래스 
- Impact.read(places=None) : Light 센서값 읽기, places 에 장소 지정하면 해당 구역만 켜기 
    - ex) {'ENTRANCE': None, 'ROOM': {'ts': 1609476471, 'state': 'none'}, 'KITCHEN': {'ts': 1609476471, 'state': 'detect'}}
---
