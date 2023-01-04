# Network Week 02

## 2. TCP/IP, LAN Driver

### 1) Socket 작성

#### (1) TCP/IP Software 구조 

- Application

  - Socket Library
    - Resolver를 내장

- OS 내부

  - TCP 프로토콜

    - 브라우저나 메일 등의 일반적인 애플리케이션의 데이터 송수신 담당

    - UDP 프로토콜
      - DNS 서버에 대한 조회 등 짧은 제어용 데이터 송수신 담당

  - IP 프로토콜

    - 패킷 송수신 동작 제어

      ```text
      패킷: 네트워크 내에서 데이터 운반 시의 분할된 덩어리
      ```

      - ICMP 

      - 패킷 운반 시 발생하는 오류 및 제어용 메시지 통지 시 사용

      - ARP

      - IP 주소에 대응하는 이더넷의 MAC 주소를 조사할 때 사용

        ```text
        MAC Address: IEEE(국제전기전자기술자협회) 에서 표준화한 LAN 방식의 기기가 가지는 고유 주소
        ```

- Driver Software

  - LAN Driver
    - LAN 어댑터의 하드웨어를 제어

- Hardware

  - LAN Adapter
    - 케이블에 대해 신호 송수신 동작 실행

#### (2) Socket

- Protocol Stack 내 제어 정보를 기록한 메모리 영역
- Protocol Stack은 Socket에 기록된 제어 정보를 참조하면서 움직임
- 각 Socket은 프로토콜 종류, Local 주소 및 포트, 원격 주소 및 포트, 통신 상태, Process ID 정보를 보유

#### (3) Socket 호출 시의 동작

- Socket 제작
  - Protocol Stack이 Socket 1개 분량의 메모리 영역 확보 
  - 초기 상태인 것을 나타내는 제어 정보와 함께 메모리 영역에 기록
- Application에 Descriptor 정보를 제공
- 이후 App은 데이터 송수신을 Protocol Stack에 의뢰할 때 디스크립터를 통지

### 2) 서버에 접속하기

#### (1) 접속의 의미

-  통신 상대와의 사이에 제어 정보를 주고 받아 소켓에 필요한 정보를 기록

-  데이터 송수신이 가능한 상태로 만드는 것

-  송수신하는 데이터를 일시적으로 저장할 '버퍼 메모리' 영역의 확보


#### (2) Header의 배치

- 제어 정보의 종류
  - **연락 절충을 위해 주고 받는 데이터 (해당 데이터를 Header에 배치)**
    - 접속 동작, 데이터 송수신 동작, 연결 끊기 동작 전부를 포함
    - 규정화된 항목들에 대해서 고정적으로 클라이언트 서버간 통신 시 패킷 맨 앞 부분에 이 정보를 부가함
    - TCP Header, Ethernet Header, IP Header 등 다양한 종류가 있기 때문에 항상 세부 기재할 것
  - 소켓에 기록하여 Protocol Stack의 동작을 제어하기 위한 정보
    - Application에서 통지된 정보, 통신 상대로부터 받은 정보를 수시 기록
    - 송수신 동작의 진행상황도 수시로 기록
    - Protocol Stack은 해당 정보를 차례대로 참조하면서 움직임 
    - 상대측에서 제어정보를 확인하는 것은 불가, 제어 정보가 필요한 것이 아니기 때문임.
    - 하지만 중요한 것은 명령에 의해 표시가 가능

#### (3) 접속 동작의 실제

- 접속 동작의 흐름

  - Socket Library의 Connect 호출

    - Descriptor, Server IP, Port Number 등 Parameter 보유

  - Protocol Stack - TCP Part로 전달, 

    - IP Address로 표시된 상대의 TCP Part와 제어 정보를 주고 받음

  - 데이터 송수신 동작의 개시를 나타내는 제어 정보를 기록한 Header 생성

    - 송수신처의 Port Number를 기록

    - SYN Control Bit를 1로 만듦

      ```text
      SYN: TCP Flag의 일종으로 Syncronization의 약자, 연결을 요청하는 Flag
      ```

  - IP Part에 전달, 송신 의뢰

  - Packet 송신 실행, Server의 IP Part가 수령 후 TCP part로 전달

  - Server의 TCP Part에서 TCP Header를 조사

    - 기록된 Port Number에 해당하는 Socket 확인
    - 해당하는 Socket은 필요한 정보를 기록, 접속 동작 진행 상태로 전환

  - Server TCP Part에서 응답 메시지를 생성

    - 마찬가지로 TCP Header 생성, ACK Control Bit를 1로 만듦

      ```text
      ACK: TCP Flag의 일종. Acknowledgemenet의 약자, 패킷의 성공적 수신 여부를 확인
      ```

  - TCP Header를 IP Part에 전달, 반송 의뢰

  - 클라이언트에 돌아온 Packet을 IP Part를 경유하여 TCP Part가 수령
  - TCP Header를 조사, 서버 접속 동작 성공 여부 체크
    - 성공했다면 Socket에 IP Address와 Port Number 등 접속 완료 제어 정보 기록

  - 서버로 ACK Control Bit 1의 TCP Header 반송 (반환 데이터 수신 확인)

- Connection: Socket이 데이터 송수신할 수 있는 상태가 됨

  - 데이터 송수신 동작이 계속되는 동안 계속적으로 존재

- Connection이 이루어지면 connect의 실행 종료, Application 제어 가능

### 3) 데이터 송수신

#### (1) 송신 데이터의 전달

- Application의 write 호출하여 Protocol Stack에 송신 데이터를 전달

  - Protocol Stack은 데이터 내용을 알 수 없으며 데이터의 길이만 인지

- Protocol Stack은 송신용 버퍼 메모리 영역에 데이터를 1차적으로 저장

  - 네트워크 이용 효율을 위한 판단 요소에 따른 송신 보류

    - 데이터의 크기 

      - 데이터의 크기를 충족할 때까지 송신 보류

      - MTU를 기반으로 Packet에 저장 가능한 데이터의 크기를 판단

      - Application에서 받은 데이터를 MSS 이하의 크기 기준으로 송신

        ```text
        MTU(Maximum Transmission Unit): 
        1 Packet에 운반할 수 있는 디지털 데이터의 최대 길이, Ethernet 기준 1500Byte.
        MSS(Maximum Segment Size): 
        1 Packet에 포함된 Header의 크기를 제외한 나머지 데이터의 최대 길이
        ```

    - 타이밍
      - 데이터 크기가 미충족 시 고려
      - Stack 내부 타이머가 일정 시간 경과한 경우 송신

  - 판단 요소를 절충한 송신 동작 실행, 따라서 OS의 종류, 버전에 따라 상이

  - Application에서의 송신 타이밍 제어가 가능, 필요에 따라 즉시 송신

#### (2) 데이터의 분할

- 일반적인 HTTP Request의 경우 1 Packet으로 충분
- Form 사용한 긴 데이터 등 1 Packet으로 충분하지 않은 경우 존재
  - 버퍼 내 데이터를 차례대로 MSS 크기에 맞춰 분할
  - 분할한 조각을 1개씩 Packet에 넣어 송신

#### (3) ACK 번호의 사용

- Data의 조각 분할 시, 각각의 조각이 몇 번째 Byte인지 기록
- TCP Header에 Sequence Number 기록
- Packet 전체 길이 - Header 길이를 통해 데이터 크기 확인 > 전체 크기 확인 가능

- N번 째 바이트까지 수신했을 때 N + 1의 Sequence Number를 확인
  - 정확하지 않다면 데이터의 누락이 발생했다는 뜻
- 수신 확인 응답
  - ACK 번호에 몇 번째 바이트까지 수신한 것인지 계산한 값을 담아 송신 측에 반송
  - 실제로는 Sequence Number 초기값으로 난수를 사용 (악의적 공격 우려)
- 양방향 통신이기 때문에 실제로는 양측에서 ACK 번호를 따로 설정, 각각 통지함
- 오류 발생 시 회복 조치가 필요 없는 강력한 구조
- 다만, 여러 가지 이유로 TCP 발송 후 데이터가 도착하지 않는 경우 데이터 송신 동작을 강제로 종료, App에 오류를 통지할 수 있음 

#### (4) ACK Number 대기 시간의 조정

- Timeout Value: ACK 번호가 돌아오는 것을 기다리는 시간
- 네트워크가 혼잡하여 정체 발생 시 ACK 번호 반환이 지연되므로 대기시간을 길게 잡아두어야 함
  - ACK 번호가 돌아오기 전에 다시 보내야 하는 사태가 발생 > 이는 네트워크 혼잡을 악화
  - 대기 시간이 너무 길어지면 패킷을 다시 보내는 동작이 지연, 속도가 저하
- 따라서 TCP는 대기 시간을 동적으로 변경하는 방식을 차용

- 항상 ACK Number의 반환 시간을 체크, 돌아오는 시간이 길어지거나 짧아지면 그에 대응하여 대기 시간도 조정

#### (5) Window 제어 방식

- 1개의 패킷을 보낸 후 대기 없이 연속으로 복수의 패킷을 보내는 방법

- ACK 번호가 반환되는 시간 동안 다음 송신을 진행하므로 낭비가 없음

- 수신 측은 수신 버퍼에 데이터를 임시 보관하면서 수신 처리를 진행

- 수신 측의 수신 가능 능력을 초과하여 패킷을 보낼 가능성이 있음 > 해결

  - Window Size를 미리 통지하는 방식으로 문제를 회피

    ```text
    Window Size: 수신 가능한 데이터 양의 최댓값, TCP 정밀 조정 Parameter 중 하나.
    ```

#### (6) ACK Number + Window

- Window 통지 타이밍

  - 수신 측에서 App에 데이터 전달 후 수신 Buffer 여유 발생
  - 해당 내용을 송신 측에 전달할 때가 타이밍

- ACK Number 통지 타이밍

  - 수신 측에서 데이터를 받아 조사 후 정상 수신을 확인할 수 있는 경우
  - 데이터 수신한 직후가 타이밍

- 종합

  ```text
  수신측 데이터 도달 - ACK 번호 통지 - APP에 데이터 전달 후 잔여 Buffer 발생 - Window 통지
  ```

  - 수신 측에서는 ACK Number, Window 통지 시 Socket 발송을 보류했다가 다음 통지 시 두 개를 1 Packet에 묶어서 발송, Packet 수를 감소시킴
  - 복수의 ACK Number 통지 발생 시 제일 마지막 번호만 통지, 나머지 생략 가능
  - 복수의 Window 통지도 마찬가지로 Buffer의 최종 잔여 영역만 통지

#### (7) HTTP 응답 메시지 수신

- 메지지 발송 이후의 동작으로 반송 메시지의 수신 대기 과정

- read 프로그램 호출
- 프로그램을 경유하여 Protocol Stack으로 제어 이동
  - 수신 Buffer에서 수신 데이터 추출, App에 전달
  - 응답 메시지 반환까지 대기 시간 동안 수신 Buffer에 데이터 추가 없음, 작업 보류
  - 응답 메시지 Packet 도착 시 수신하여 App에 전달하는 작업을 재개
- Protocol Stack의 수신 동작 정리
  - Data Segment와 TCP Header를 조사, 데이터 누락 검토
  - 문제가 없었다면 ACK Number 반송
  - Data Segment를 연결하여 원래 모습으로 복원 후 App이 지정한 메모리 영역에 기록, App에 제어를 반환
  - 데이터 전달 후 타이밍에 맞춰 Window 통지

### 4) 연결 끊기 & Socket 말소

#### (1) 데이터 보내기 완료 후 연결 끊기

- App이 송신해야 하는 데이터를 전부 송신 완료했다고 판단했을 때 진행
- 송신 완료한 측에서 연결 끊기 단계 돌입
- App에 따라서 데이터 송수신 동작 종료 관점이 다름
  - Web의 경우, 응답 메시지를 반송 완료하는 Server 측에서 돌입 시작
  - Client 측에서 먼저 완료하는 경우 Client 측에서 시작

- 단계 설명 (Server side 시작)
  - Server 측 App 이 Socket Library - close 호출
  - Server Protocol Stack이 TCP Header 생성, 연결 끊기 정보를 설정
    - Control Bit FIN 을 1로 설정
    - IP Part에 클라이언트로의 송신을 의뢰
  - Server Socket에 연결 끊기 동작에 돌입했다는 정보를 기록
  - Client에 FIN이 1로 설정된 TCP Header 도착
  - Client Socket에 연결 끊기 동작에 돌입했다는 정보를 기록
  - 패킷 정상 수령을 통지하는 ACK Number 반송 후 App의 데이터 수령 대기
  - App이 read를 통해 데이터를 받으러 왔을 때 데이터 대신 데이터 수신 완료 사실을 통보
  - Client App에서 close 호출로 데이터 송수신을 종료
  - Client Protocol Stack에서도 서버와 마찬가지로 FIN Bit를 1로 설정한 TCP Header 생성 후 IP Part에 송신 의뢰
  - 서버로부터 ACK Number가 반환되는 것으로 종료

#### (2) Socket 말소

- 오동작의 예방을 위해 서버와의 대화 종료 후 바로 Socket의 제거를 진행하지 않음
- 서버에서 FIN을 여러 번 보냈을 때 Socket이 사라졌다가 새로운 App에 같은 번호로 할당되었을 경우 연결 끊기에 돌입할 수 있음
- Packet이 없어졌을 때 다시 보내는 동작이 몇 분 동안 지속되므로 해당 시간 동안 대기 후 Socket을 말소하게 됨

#### (3) 데이터 송수신 동작 정리

- 접속 동작
- 송수신 동작
- 연결 끊기 동작

### 5) IP 및 Ethernet의 Packet 송수신

#### (1) Packet의 기본

- 구성
  - Header
    - 수신처 주소 등의 제어 정보를 포함
  - Data
    - 의뢰처에서 의뢰한 정보를 포함

- TCP/IP의 Packet

  - Packet
    - TCP Header + Data로 구성
  - IP Header
    - IP의 제어 정보

  - MAC Header
    - Ethernet 제어 정보

- Packet 전달 단계
  - 송신처에서 Packet 생성
  - 가장 가까운 중계 장치에 송신
  - 중계 장치에서 Header 조사를 통해 목적지를 판단
  - 여러 중계 장치를 거쳐 수신처 기기에 Packet 도달
  - 수신처에서 회답 Packet 반송
    - End Node: 일반적으로 송수신이 양방향으로 발생하므로 송수신처를 명확하게 구별하지 않고 묶어서 부르는 명칭

- TCP/IP Packet의 MAC Header와 IP Header가 역할을 분담
  - IP Header: 최종 목적지, 변동 없음
  - MAC Header: 다음 라우터, 라우터를 이동할 때마다 달라짐
- Ethernet의 역할은 다른 것으로 대체가 가능 (무선 LAN, ADSL 등)
  - 유연한 네트워크 구축을 위한 역할 분담

#### (2) Packet 송수신 동작 개요

- TCP Part에서 IP Part에 Packet 송신 의뢰
  - Data Segment에 TCP Header를 부가하여 IP Part에 전달
  - Packet 내용물 및 최종 IP Address 정보
- IP Part에서 IP Header와 MAC Header 부가
  - IP Header
    - IP Protocol에 규정된 규칙에 따라 IP Address까지 Packet을 전달할 때 사용하는 제어 정보
  - MAC Header
    - Ethernet 등의 LAN을 사용하여 가장 가까운 Router까지 Packet을 운반할 때 사용하는 정보
- Network Hardware에 전달
  - 0과 1로 이루어진 비트의 연속체
  - 전기, 빛 신호 등으로 케이블 송출
- 중계 장치에 도착, 중계 장치가 상대가 있는 곳까지 Packet을 전달
- 수신처 Network Hardware에서 신호를 디지털 데이터로 역변환
- 역변환된 Packet을 IP Part로 전달
- TCP Part가 TCP Header + Data로 이루어진 Packet을 수령

#### (3) IP Header의 생성

- IP Part의 역할

  - IP Header의 생성 및 Packet에 IP Header를 부가

  - 수신처 IP Address
    - App이 설정한 주소를 기반으로 그대로 Packet 송신
  - 송신처 IP Address
    - LAN Adapter에 할당된 주소를 기반으로 설정 (복수 LAN Adapter의 가능성)
  - Packet을 건네줄 상대 판단
    - Router가 하는 역할과 동일
    - IP 규칙에 따라서 Packet 송수신

- 경로표의 사용

  - Network Destination과 수신처 IP 주소를 비교
  - 송신 가능 여부와 Gateway IP 주소를 확인
  - 경로표에 일치하는 것이 없는 경우 기본 Gateway를 사용

- Protocol Number Field 값 설정

  - Packet 내용물의 의뢰처를 확인할 수 있는 값을 설정

    ```text
    TCP: 06, UDP: 17 (전부 16진수) 등으로 규칙에 따라서 이미 정해진 값을 사용
    ```

#### (4) MAC Header의 생성

- Ethernet은 TCP/IP와 다른 구조로 Packet의 수신처를 판단
- Ethernet의 수신처 판단 시 사용하는 것이 MAC Header
  - MAC Header는 IP 주소와 달리 48비트로 구성
  - 48비트가 하나의 값을 나타냄
- Header 설정
  - Ether Type
    - IP Protocol을 나타내는 값을 설정
  - 송신처 MAC Address
    - LAN Adapter의 MAC Address 설정
    - 해당 주소는 Adapter 제조 시 ROM에 기록됨
  - 수신처 MAC Address
    - 전달할 상대의 MAC 주소
    - 경로표를 참조하여 Gateway로 지정된 기기를 확인
    - Gateway에 지정된 상대의 IP만 알고 있기 때문에 MAC 주소 조사 동작 필요

#### (5) ARP를 통해 MAC 주소 조사

- ARP: Address Resolution Protocol

- Ethernet 내 전원에게 Packet을 전달하는 Broadcast 구조를 활용
- 같은 네트워크에 존재한다면 MAC 주소를 알 수 있음
- 계속 반복 동작을 하게 된다면 Packet이 늘어나므로 Cache 메모리에 조사한 결과를 보존, 재이용
- 캐시에 저장된 값이 현실과 일치하지 않을 수 있기 때문에 Cache에 보존된 데이터는 몇 분이 지나면 삭제

#### (6) Ethernet의 기본

- 10BASE5
  - 이더넷의 원형
  - 케이블을 통해 신호가 전체에 전달
- 10BASE-T
  - Repeater Hub에서 흩뿌려진 데이터가 전체에 전달
- Switching Hub를 이용한 형태
  - MAC Address의 주소에 따라 목적지 확인 후 Packet 중계
  - 신호가 원하는 상대에게 전달

#### (7) IP Packet을 전기, 빛 신호로 변환하여 송신

- LAN Adapter의 역할

- Driver Software를 통한 제어가 필요함

- Ethernet 송수신 제어 회로에 MAC 주소를 설정

  ```text
  MAC: Media Access Control의 약자
  ```

- 보통은 ROM에서 중복될 수 없는 MAC Address 주소를 받아와서 사용

- 용도에 따라서 명령 및 설정 파일을 통해 설정하는 경우, ROM 기록을 무시

#### (8) Packet에 제어용 데이터를 추가

- Preamble
  - 1과 0이 번갈아 나타나는 비트열이 56비트 이어진 것
  - 비트 패턴 신호 파형을 통해 Timing을 판단할 때 사용
  
- Start Frame Delimiter
  - 끝이 11인 비트 패턴으로 이로 인해 파형에 변화가 발생함
  - Frame 개시 위치를 확인하는 용도
- Clock
  - 해당 신호가 위아래로 변동할 때 데이터의 전압 혹은 전류의 값을 읽고 대응할 수 있도록 하는 별도의 추가 신호
  - 전달 시간에 따라서 클록이 틀어져버리는 문제가 발생
  - 따라서 데이터 신호와 클록 신호를 합성한 하나의 신호를 송신

- Frame Check Sequence
  - 오류 검출용 데이터
  - 파형이 흐트러져 데이터의 변경이 발생한 경우를 검출하기 위해 사용
  - 디스크에 사용하는 오류 검사 코드와 동일한 형식, 값이 1비트라도 변화할 경우 값이 달라짐

#### (9) 허브로 Packet 송신

- 반이중 모드 (Repeater Hub 사용)
  - 송수신을 동시에 병행할 수 없는 경우
- 전이중 모드 (Switching Hub 사용)
  - 송수신을 동시에 병행할 수 있는 경우
- 반이중 모드의 동작
  - 케이블에 다른 기기가 송신한 신호가 없는지 조사 후 송신 시작
  - MAC 회로가 Preamble 맨 앞부터 데이터 변환 후 PHY 혹은 MAU 라는 송수신 신호 회로에 보냄
  - PHY(MAU) 회로가 케이블에 적절한 형식으로 송신
  - 낮은 확률로 복수의 신호가 송신에 들어가는 경우 충돌 발생
  - 충돌이 발생한 경우 재밍 신호를 흘린 뒤 송신 정지 얼마간 대기 후 송신 재개
  - 대기 시간이 중복되지 않도록 난수 대기 시간을 부여

#### (10) Packet 회신

- 데이터를 수신한 후 파형의 타이밍을 계산, 데이터를 변환

- PHY(MAU)회로에서 공통 형식으로 변환 후 MAC 회로를 통해 디지털 데이터로 변환, 버퍼 메모리에 저장

- FCS 검사를 통해 데이터의 오류를 확인

- 문제가 없을 경우 수신처 MAC Address를 조사, 자신에게 온 것이 맞을 경우에만 Packet을 받아서 버퍼 메모리에 저장

- Packet 수신을 컴퓨터 본체에 통지

- Interrupt 구조

  - 송수신하는 동안 본체는 다른 작업을 실행하는 중

  - 작업에 끼어들어 LAN Adapter에 주의시키는 것

  - 확장 BUS 슬롯의 Interrupt 신호선에 신호 송신, Interrupt Controller 를 통해 CPU에 연결

  - 신호 수신 시 CPU 실행 작업 보류 후 Interrupt 처리 프로그램으로 전환, LAN Driver를 호출하여 송수신 실행

  - 기존에는 Interrupt 번호 설정이 필요했으나 현재는 PnP 사양에 따른 자동 결정으로 번호 설정 불필요

    ```text
    PnP: Plug and Play, 확장 보드나 주변 기기 등을 자동으로 설정하는 기능
    ```

#### (11) Server 응답 Packet을 IP에서 TCP에 전달

- 서버에서 반송된 Packet 타입을 확인, LAN Driver 가 TCP/IP의 Protocol Stack에 Packet 전달
- IP 부분에서 IP Header 조사를 통해 Format 및 수신처 IP Address 확인
- 수신처가 자신의 IP와 다른 경우 IP 담당이 ICMP라는 메시지를 사용, 오류를 통지함
- 만약 Packet이 분할된 내용일 경우 원래 Packet으로 복구를 위해 나머지 분할 Packet의 도착을 대기
- 모든 ID 정보가 같은 Packet을 합쳐 복구한 뒤 이것을 참조 (Fragment offset 항목을 통해 reassembling)
- 이후 통신 진행 상황에 따라 적절한 동작을 실행

### 6. UDP Protocol을 통한 송수신

#### (1) 수정 송신이 필요없는 데이터의 송신

- TCP가 복잡한 이유는 오류로 도착하지 못한 패킷을 송신할 수 있는 구조이기 때문
- TCP를 사용하지 않는다면 접속 제어용 Packet, 수신 확인 Packet이 필요하지 않음

#### (2) 제어용 짧은 데이터

- 1개의 Packet으로 끝나는 경우가 많음

#### (3) 음성 및 동영상 데이터

- 정해진 시간 안에 데이터를 전달해야 함
- 지연된 데이터는 쓸모가 없음
- 데이터가 다소 부족해도 치명적이지 않음

