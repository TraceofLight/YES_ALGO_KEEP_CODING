# Network Week 04

## 4. Access Line & Provider

### 1) ADSL 기술을 이용한 Access 회선의 구조와 동작

#### (4) ADSL의 변조 방식 신호화

- LAN과 달리 완만한 파형을 합성한 신호에 0과 1의 비트값을 대응시키는 기술을 사용 (변조 기술)

- 진폭 변조 (ASK, Amplitude Shift keying) + 위상 변조 (PSK, Phase Shift Keying) 인 **직교 진폭 변조 (QAM, Quadrature Amplitude Modulation)** 방식을 사용

- 진폭 변조

  - 신호의 진폭의 크기에 0과 1을 대응시키는 방법
  - 진폭의 단계를 분할하여 데이터 전달의 고속화가 가능
  - 전달 중 감쇠 등을 통해 파형이 변형되어 오류의 원인이 되므로 적정 수준의 단계 수만 사용

- 위상 변조

  - 신호의 위상에 0과 1을 대응시키는 방법

    ```text
    파는 순환하는 주기의 시작점에 따라 모양이 다르며, 파가 원래의 형태로 돌아가기까지의 기간을 1주기로 한다.
    위상: 1주기 동안의 어느 시점을 각도에 대응하여 나타내는 것
    ```

  - 각도의 단계를 세분화하여 데이터 전달 고속화가 가능

  - 각도가 가까워질수록 판별에 어려움, 고속화에 한계 존재

- 직교 진폭 변조

  - 진폭과 위상에 각각 1비트를 대응
  - 1개의 파에 2비트 분의 데이터가 대응하므로 상대적으로 고속화된 데이터 송신 가능

#### (5) 파를 많이 사용한 고속화의 실현

- 주파수가 서로 다른 파를 혼합하여 파의 합성이 가능
- 특정 주파수의 파만 통과시키는 필터 회로를 통해 주파수마다 파를 분리할 수 있음
- 각 파마다 데이터를 담고 이를 합성하여 전송하는 것을 통해 많은 데이터를 보낼 수 있음
- ADSL의 데이터 고속화
  - 4.3125kHz 씩 주파수를 다르게 한 수백 개의 파를 사용
  - 각 파에 직교 진폭 변조 방식을 통해 비트값을 대응
  - 잡음 등의 상태에 따라 담을 비트 수를 조절

- ADSL 의 업로드와 다운로드 속도 차이가 나는 이유
  - 업로드는 26개의 파를 사용 / 다운로드는 95개나 223개의 파를 사용
- 트레이닝
  - 주파수가 높아질수록 잡음의 영향력과 감쇠가 발생하는 정도가 커서 적은 비트수만 지원하는 경우가 많음
  - 시험 신호를 보내어 수신 상태에 따라 대응할 파의 수, 비트 수를 판단함

#### (6) Splitter의 역할

- 사용자 측의 신호를 전화 회선으로 송출하는 경우
  - ADSL 모뎀에서 전기신호로 변환된 Cell은 Splitter로 들어가고 전화의 음성 신호와 섞여서 전화 회선에 함께 흘러나감.
  - 그대로 흘리기만 하며 특별하게 하는 일이 없음
- 전화 회선에서 신호가 흘러들어온 경우
  - 전화와 ADSL의 신호를 나누는 역할
  - 일정 주파수를 초과하는 신호를 차단하여 ADSL의 높은 주파수 신호를 차단
  - ADSL 내부에 불필요 주파수 차단 기능이 포함되어 전화 주파수를 내보내면서 따로 차단할 필요는 없음
- Splitter는 ADSL과 전화기 양측에 서로 미치는 영향을 차단하기 위해 사용
  - Splitter가 없다면 신호를 전달하는 쪽이 바뀌기 때문에 이에 따라 잡음 등의 성질이 바뀜
  - 성질이 바뀌게 된다면 추가적인 트레이닝을 통해 확인을 진행하게 되므로 통신의 중단이 발생
  - 고속 트레이닝을 하는 경우 스플리터가 없어도 문제가 없다는 개념도 존재하지만, 전화 소리를 듣기 어렵다는 이유로 Splitter 사용 (G.992.2 ADSL 사양)

#### (7) 전화국까지의 여정

- Splitter 를 지나  module connector 를 통과
- IDF, MDF 등의 배선반
- 낙뢰 등으로부터 보호하는 보안기
- 전주의 전화 케이블
- 지하 케이블 (동도)
- 전화국의 MDF

#### (8) 잡음의 영향

- Cross Talk
  - 케이블 내부에서 발생하는 잡음
  - Ethernet의 Twist Cable 에서도 발생하지만 더 쉽게 잡음의 영향을 받음
- ADSL의 신호는 다수의 주파수로 나뉘어 있어 잡음과 겹치는 주파수만 영향을 받아 사용불가가 됨 &rarr; 속도 저하
- 잡음이 많은 장소에 전화 케이블이 가설된 경우 속도가 저하될 가능성이 있음
- 주변 ISDN 회선에서 누설되는 잡음에 영향을 받았으나 현재는 방지 기술이 확립되어 주의 필요성 감소

#### (9) DSLAM을 통과하여 BAS로

- 전화국에 도달한 신호가 배선반, Splitter를 통과하여 DSLAM에 도달

  ```text
  DSLAM: DSL Access Multiplexer, 전화국용 ADSL 집합 모뎀
  ```

- 전기 신호가 Digital Data Cell로 복원

  - DSLAM이 신호의 파형을 읽어 진폭과 위상을 조사, 대응하는 비트값으로 Digital Data 화

- DSLAM은 ATM Interface를 보유, 분할한 Cell 형태를 그대로 후방 Router와 주고 받음

- DSLAM을 나와 BAS라는 Packet 중계 장치에 도착

  ```text
  BAS: Broadband Access Server
  ```

- Cell을 원래의 Packet으로 복원 

- 필요 없는 Header 제거 및 Tunneling 용 Header를 붙여 Tunneling 출구로 중계

- Tunneling 용 Header 제거 후 IP Packet 을 추출하여 인터넷의 내부에 중계

### 2) 광섬유를 이용한 Access Line

#### (1) 광섬유의 기본

- 이중 구조의 가는 섬유질의 투명한 재질 (유리나 플라스틱)
- 전기신호를 광신호로 변환하여 광원에 입력하면 광섬유 안에서 전달된 신호가 수신측에 도착
- 수광 소자를 통해 빛의 밝기에 따라 전압을 일으킬 수 있으며 이것을 Digital Data로 변환

#### (2) 싱글모드와 멀티모드의 차이

- 빛의 굴절률을 활용한 Core & Clad 사이의 전반사를 활용한 것이 광섬유
- 코어에 직경에 따라 싱글모드와 멀티모드를 구분
- 싱글모드
  - 코어가 가늘고 입사각이 작은 빛만 투입 가능
  - 코어 속을 진행하는 빛이 한 개 밖에 없으므로 신호 변형이 적음
  - 멀티모드보다 케이블 길이를 길게 할 수 있음, 멀리 떨어진 장소에 있는 건물 사이 연결 시 사용
- 멀티모드
  - 반사각이 다른 몇 개의 빛이 코어 속을 진행
  - 반사횟수가 많은 경우 코스의 거리가 길어지면서 도착 시간에 영향, 신호에 변형을 주게 됨
  - 허용 한도를 넘은 경우 통신 오류가 발생
  - 건물 내에서 연결하는 것에 사용

#### (3) 광섬유를 분기시켜서 비용을 절감

- ADSL 대신 광섬유를 사용하는 것이 FTTH Access Line

- Packet 에는 다른 점이 없기 때문에 PPPoE를 이용하여 Packet을 운반했을 경우의 동작은 ADSL과 동일함

- 1개의 광섬유로 사용자측과 가장 가까운 전화국측을 접속하는 유형

  - 사용자측 Media Converter 에서 Ethernet 전기 신호를 광신호로 변환

  - 광신호는 바로 앞의 집합형 Media Converter로 흘러감

  - 전기 신호로 복원, BAS Port가 수신, 인터넷 내부로 Packet 중계

  - 상향 광신호와 하향 광신호는 파장을 변화시켜 구별 (파장 다중)

    ```text
    파장 다중: 1개의 광섬유에 파장이 서로 다른 복수의 광신호를 흘림
    ```

- 사용자 부근의 전주에 광스플리터 분기 장치를 설치, 광섬유를 분기시켜 복수의 사용자를 연결하는 유형

  - ONU를 사용자측에 설치하고 Ethernet 신호를 광신호로 변환

    ```text
    ONU: Optical Network Unit, 종단 장치라고도 불림, 전기 신호를 광신호로 변환하는 장치 + 신호의 충돌을 방지하는 기능 보유
    ```

  - BAS 바로 앞 OLT라는 장치로 신호를 흘림

    ```text
    OLT: Optical Line Terminal
    ```

  - 복수의 사용자가 동시에 Packet 송신 동작을 실행하게 되면 광신호의 충돌이 발생하기 때문에 타이밍을 조절하는 기능이 OLT와 ONU에 탑재되어 있음

  - OLT가 ONU에 송신 지시를 내리며 ONU가 송신 동작을 실행하는 형태로 작동

### 3) Access Line으로 이용하는 PPP와 터널링

#### (1) 본인 확인과 설정 정보를 통지

- 최초에 User Name과 Password를 입력하여 로그인을 실행하지 않으면 인터넷에 Access가 불가능

- BAS가 로그인 창구 역할이며, 이 역할의 실현을 위해 PPPoE라는 구조를 사용

- PPPoE: 보통의 전화회선을 이용한 Dial-up 접속으로 이용하는 PPP라는 구조를 발전시킨 것

- PPP의 구조

  - Provider의 Access Point에 전화

  - 전화가 연결된 경우, User Name과 Password를 입력하여 로그인 조작

  - RADIUS라는 Protocol을 사용, RAS에서 본인 확인용 인증 서버에 전송, 정확한지 검사

    ```text
    RADIUS: Romote Authentication Dial-In User Service
    RAS: Remote Access Server
    ```

  - Password 확인되면 설정 정보를 사용자측으로 반송

  - 사용자는 정보를 확인하고 IP Address 등을 설정, TCP/IP Packet 송신 준비 완료

  - Packet 송수신 단계

- Dial-up Access는 전화번호에 따라 Access Point 전환이 가능, 전환된 Access Point에 따라 주소가 달라짐.

- 사전에 PC 주소를 고정할 수 없기 때문에 접속되었을 때 PC의 TCP/IP 설정 정보를 통지, 글로벌 주소를 PC에 설정

#### (2) Ethernet에서 PPP Message를 주고 받는 PPPoE

- ADSL, FTTH에 PPP를 사용해야 하는 이유

  - ADSL, FTTH도 PC에 Global Address를 설정하지 않으면 인터넷에 접속할 수 없음

  - 사용자와 BAS를 고정적으로 접속, 본인 확인이 필요가 없기 때문에 PPP 구조가 전부 필요한 것은 아님

  - User Name, Password 입력 동작을 남겨두면 사용자명에 따른 Provider를 전환할 수 있어서 편리

- PPP Message를 운반할 때는 IP Packet을 Ethernet Packet에 넣어 운반하는 개념이 필요

- PPP Protocol에 해당 규정이나 신호 규정이 존재하지 않아 규정을 가진 '그릇'을 준비하고 여기에 저장 &rarr;`HDLC`Protocol 사용

  ```text
  HDLC: High-level Data Link Control, 원래 전용선 통신 회선을 사용하기 위한 Packet 운반을 위해 만들어졌고 이를 Dial-up 접속에서 일부 수정하여 사용
  ```

- ADSL, FTTH는 HDLC의 사용이 불가하므로 Ethernet Packet을 그릇으로 사용하기로 함

- Ethernet은 PPP와 다른 부분이 있어 격차를 메울 사양으로 PPPoE Protocol을 생성

#### (3) Tunneling 기능을 통한 Packet 전달 to Provider

- Tunneling: Socket과 Socket 사이를 연결하는 TCP Connection과 유사
- BAS와 Provider 사이에 있는 ADSL/FTTH 접속 서비스 사업자의 Network 내부에 Tunnel 생성
- Tunnel에 회선을 연결하면 Packet이 그 경로를 통해 들어가는 형태가 됨
- TCP Connection을 사용하여 실현하는 방법
  - Tunneling Router 사이에 TCP Connection 생성
  - Connection 양 끝 Socket을 Router Port로 간주, Packet을 송수신

- 캡슐화 방법을 사용하여 실현하는 방법
  - Header를 포함한 Packet 전체를 별도 Packet 안에 저장
  - Tunneling을 통해 운반
- Packet을 그대로의 모습으로 운반할 수 있는 구조라면 원리적으로는 어떤 것이든 Tunneling의 이용이 가능

#### (4) Access Line 전체 동작

- 인터넷 접속용 Router에 Provider가 할당한 User Name과 Password를 등록
- PPPoE의 Discovery라는 구조를 따라 BAS 탐색 (ARP와 같이 인터넷의 BroadCast를 이용)
- BAS의 MAC Address를 확인 후 BAS와 대화
- Password를 2가지 방식을 통해 송신
  - CHAP: Challenge Handshake Authentication Protocol, Password를 암호화 하는 방식
  - PAP: Password Authentication Protocol, 암호화 하지 않는 방식
- BAS에서 TCP/IP 설정 정보를 사용자에게 통지
  - IP Address, DNS Server IP Address, Gateway Address 를 통지받음
  - 인터넷 접속용 Router를 사용할 경우 이 설정 정보를 Router 자체에 설정
  - Router의 BAS측 Port에 Global Address가 할당, 경로표에 기본 Gateway도 설정
- Packet을 인터넷에 중계할 수 있는 상태 구성 완료
- Client로부터 인터넷에 액세스하는 Packet이 흘러 들어옴
  - 대부분 인터넷 접속용 Router에는 등록되어 있지 않을 것
  - 기본 경로를 채택하여 BAS에서 통지한 기본 Gateway에 Packet 중계
- PPPoE 규칙에 따라서 Port에서 Packet 송신
  - MAC Header
    - 수신처 MAC Address: PPPoE - Discovery 에서 탐색한 BAS MAC Address
    - 송신처 MAC Address: 인터넷 접속용 Router의 BAS측 Port의 MAC Address
  - PPPoE Header & PPP Header
    - Payload Length를 제외한 값들은 사전에 결정
    - Payload Length는 Packet의 길이를 조사하면 알 수 있음
- BAS에 도착 후 Packet에서 MAC & PPPoE Header를 제거, PPP Header 이후 부분 추출
- Tunneling을 통한 Packet 송신, Provider의 Router에 도착

#### (5) IP Address를 할당하지 않는 Un-Numbered

- Header를 부가할 때 대부분의 값은 사전에 결정 &rarr; 경로표의 기본 Gateway 항목에 어떤 값이 들어와도 관계가 없음
- Router의 Port끼리 하나의 Cable로 연결하는 상태인 경우 한 쪽에서 송신한 Packet은 반드시 반대편에 도달 (1 : 1의 상태)
- 중계 대상의 주소를 조사하는 동작 필요 없음, Gateway 항목에 값을 기록할 필요 없음, 중계 대상 Router Port IP Address 할당 필요 없음
- 그 전까지는 규칙으로 인해 Port에 항상 주소를 할당해왔으나 Global Address가 부족해졌을 경우를 고려한 특례 마련 &rarr; 1 : 1 접속의 경우 IP Address를 할당하지 않음 
- BAS에서 설정 정보를 통지할 때 기본 Gateway IP Address를 통지하지 않음

#### (6) 인터넷 접속용 Router에서 Private Address를 Global Adress로 변환

- BAS는 사용자측에 TCP/IP의 설정 정보를 통지, PC에 설정할 경우 주소 변환을 사용하지 않고 인터넷 Access가 가능
- 인터넷 접속용 Router를 사용하는 경우 설정 정보를 Router가 받음 &rarr; PC에 Global Address를 할당할 수 없음
- PC에서 보낸 Packet을 Router에서 주소 변환 후 인터넷에 중계하는 방식
- App에 따라서 주소 변환의 영향으로 정상 작동하지 않는 케이스가 있으므로 주의 &rarr; Router를 사용하지 않는 본래의 방법으로 해결
- 직접 연결의 경우 공격 받을 가능성을 염두에 두고 방화벽의 사용 등 조치가 필요

#### (7) PPPoE 이외의 방식

- PPPoA
  - ADSL Access Line의 일종
  - MAC Header 및 PPPoE Header를 붙이지 않아 PPP Message를 그대로 전송
  - Ethernet에 PPP Message를 그대로 전송할 수 있도록 ADSL 모뎀과 기기를 일체화 해야 함
    - PC의 USB Interface에 ADSL 모뎀을 연결: 보급되지 않고 종료
    - Modem과 Router를 일체화시킨 ADSL Modem을 사용 &rarr; Router 없이 인터넷에 연결하는 대책이 없어 주소 변환 문제 발생 시 곤란
  - Header 부가 횟수가 적어 MTU가 짧아지지 않음
- DHCP
  - Dynamic Host Configuration Protocol
  - PPP를 사용하지 않는 방식
  - 사내 LAN에서 주로 사용하며, User Name 및 Password 확인 절차를 거치지 않음
  - Provider를 전환하는 것이 불가능하지만 Header 없이 Packet만 주고 받을 수 있기 때문에 MTU가 짧아지지 않음
  - Cell도 사용하지 않으며 Ethernet Packet을 그대로 ADSL 신호로 변환, 따라서 Modem과 Router 분리 불가능 제약도 없음

### 4) Provider의 내부

#### (1) POP & NOC

- 인터넷은 다수의 Provider의 Network를 서로 접속한 것
- ADSL, FTTH의 Access Line은 사용자가 계약한 Provider의 POP에 연결
- POP: Point Of Presence
  - 여러 가지 유형의 Router가 설치되어 있음
  - 역할에 따른 Router 구분 사용
    - 전용선을 이용한 Access Line: 통신 회선용 Port를 장착한 보통의 Router
    - 전화 회선, ISDN 회선: RAS Router 사용
    - ADSL, FTTH: BAS와 연결하기 위한 Router
      - PPPoE: 본인 확인이나 설정 정보 통지의 동작은 BAS가 창구 역할, 따라서 중계 역할만 하는 보통의 Router 사용
      - PPPoA: DSLAM에서 ATM Switch를 경유하여 ADSL 사업자 BAS에 연결
  - Access Line 측 Router: Port는 다수가 필요하지만 인터넷 중심부 Line과 비교할 경우 속도가 느리므로 포트당 가격을 줄인 기종이 적합
  - NOC 측 Router: 다수의 Router에서 나오는 Packet이 모이는 장소, 사용하는 회선의 속도도 빠르므로 Packet 중계 능력이나 Data 전송 능력이 높은 기종이 적합
- NOC: Network Operation Center
  - Provider의 핵심이 되는 설비
  - POP에서 들어온 Packet이 여기로 집결, 이후 목적지 근처 POP 혹은 다른 Provider로 Packet이 흘러감 (개인용 대비 10000배 이상의 고성능 Router 설치)
  - POP과 엄밀하게 구분할 수는 없으며 Packet 중계하는 기본 동작은 모든 Router에서 동일하므로 규모의 차이로 보는 것이 적합

#### (2) 건물 밖에서 통신 회선에 접속하는 방법

- 건물 안에서는 케이블을 통해 접속
- 멀리 떨어진 장소에 있는 NOC와 POP를 연결하는 방법
  - 광섬유를 사용하여 연결하는 방식
    - 지중 가설 등의 공사로 인한 비용, 케이블 유지 보수 비용 등 막대한 비용의 발생
    - 따라서 극소수의 대형 Provider 들만 가능한 방법
  - 광섬유를 대여하는 방식
    - 광섬유 미보유 Provider는 통신 회선을 사용하여 원거리 NOC, POP를 연결
    - 다양한 방식의 통신 회선을 제공 혹은 광섬유를 그대로 대출하는 서비스도 존재

### 5) Provider 를 경유하여 흐르는 Packet

#### (1) Provider끼리의 접속

- 송수신 양측이 같은 Provider에 접속
  - Router끼리 경로 정보를 교환하고 경로표에 자동 등록
  - Packet은 특이 사항 없이 수신 측에 도달
- 양측의 Provider가 다른 경우
  - Provider끼리 경로 정보를 교환, 다른 Provider의 경로 정보도 보유하고 있음
  - 따라서 Router에는 항상 모든 경로가 등록된 상태
  - Provider가 달라도 정상적으로 수신 측에 도달이 가능

#### (2) Provider끼리 경로 정보 교환

- BGP (Border Gateway Protocol)

  - 접속 상대가 경로 정보를 가르쳐 주면 그 정보를 경로표에 등록

  - 같은 방식으로 상대한테도 경로 정보를 통지
  - Router가 자동적으로 수행

- Transit

  - 인터넷 경로를 전부 상대방한테 통지
  - 해당 Provider를 통해 인터넷 전체에 Packet 송신 가능

- Peer

  - 두 Provider가 서로의 Network 경로 정보만 상대에게 통지
  - 서로의 Network로 갈 Packet만 흐르게 됨

#### (3) 사내 네트워크에서의 정보 교환 및 Provider가 해당 방식을 사용할 수 없는 이유

- 기본적으로 Router끼리 정보를 교환하여 경로표를 자동으로 설정하는 방법을 사용
- 목적지까지의 최단 경로를 찾아 Packet을 중계하는 방식으로 만들어져 주변 Router들과 경로를 무차별적으로 교환
- 해당 방식을 Provider끼리의 정보 교환에 사용할 수 없음 (비용 부담에 응하지 않는 Provider로부터 흘러드는 Packet을 중지할 수 없기 때문)
- 인터넷의 경로 정보 교환 방식
  - 경로 정보 교환 상대 지정 가능
  - 경로 정보를 교환하지 않은 상대에게는 Packet 전달이 불가능하지만 그러한 사태가 일어나지 않도록 고려하여 정보 교환을 실행

#### (4) IX의 필요성

```text
IX: Internet eXchange
```

- Provider끼리 1 : 1의 접속을 사용하려면 모든 Provider들과 통신 회선을 연결해야 함
- 중심 설비 IX를 설치, 해당 설비를 경유하여 접속하는 방식을 통해 통신 회선의 수를 억제

#### (5) IX에서 Provider간의 접속

- IX는 정전이나 화재, 지진 등의 재해가 있어도 Router의 Network 기기가 정지하지 않도록 자가 발전 설비를 갖춘 내진 구조 건물 안에 존재
- Provider의 NIC도 마찬가지의 입지를 요구하며 해당 건물들에 대개 NOC 혹은  IX가 위치함
- 건물 내의 IX의 중심에는 고속 LAN의 Interface를 다수 장착한 Layer 2 Switch(고속 스위칭 허브)가 존재
- Provider의 Router를 연결
  - NOC에서 광섬유 케이블을 가설하여 IX의 Switch에 접속
  - Router를 IX에 가지고 들어가서 Switch에 접속
  - 통신 회선, 다크 파이버 등으로 Switch에 접속
  - 트래픽이 집중된 곳에 설치한 IX 부설 Switch에 접속
