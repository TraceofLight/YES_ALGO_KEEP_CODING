# Network Week 1

## 1. Web Browser

### 1) HTTP Request Message

#### (1) URL 입력

-  URL: Uniform Resource Locator

  - 브라우저는 여러 개의 클라이언트 기능을 겸비한 소프트웨어

  - http 뿐만 아니라 ftp, file, mailto 등의 다양한 프로토콜을 보유

    ```text
    프로토콜: 통신 동작의 규칙
    ex) FTP Protocol: File Transfer 통신을 위한 동작 규칙
    ```

  - 도메인명, 경로명, 상대 메일 주소, 사용자명, 패스워드, 포트 번호 등을 포함

- URL의 맨 앞에 엑세스 방법인 "프로토콜" 을 표기

#### (2) URL의 해독

- 브라우저는 Request Message를 작성하기 위해 URL을 해독
- URL의 요소
  - 프로토콜
  - "//" : 이어지는 문자열이 서버의 이름일 경우 표기
  - 웹 서버명
  - 디렉토리명
  - 파일명

#### (3) URL 일부 생략

- 파일명의 생략: 디렉토리 이하의 서버측에서 설정한 기본 파일에 엑세스
- 디렉토리명까지 생략: 루트 디렉토리의 서버측에서 설정한 기본 파일에 엑세스
- / 의 여부: /가 없다면 해당 이름이 디렉토리명인지 파일명인지 알 수 없으므로 탐색으로 확인

#### (4) HTTP 프로토콜

- 클라이언트와 서버의 상호작용을 정의

- 클라이언트로부터 서버를 향해 Request Message

  - URI(Uniform Request Identifier): 해당 Request Message의 처리 대상

    ```text
    CGI: 프로그램 호출 규칙
    CGI Program: CGI의 규칙에 맞도록 움직이는 프로그램
    
    보통 페이지 데이터 저장 파일명 혹은 CGI 프로그램 파일명을 URI로 사용
    ```

  - Method: 처리 대상에 대한 동작을 요구

    ```text
    GET, POST, HEAD, OPTIONS, PUT, DELETE, TRACE, CONNECT
    ```

- 서버는 Request Message를 해독하고 요구에 따라 동작, 이후 결과 데이터(+ 스테이터스 코드)를 응답 메시지에 저장
- 해당 메시지를 클라이언트에 반송
- 클라이언트는 반송된 메시지에서 데이터를 추출하여 화면에 표시하게 됨

#### (5) Request Message

- Request Message: Request Line + Message Header + Main Message

  - Request를 보낼 때 중요한 것은 메소드, 어떤 메소드를 사용해야 할지 판단해야 함
    - URL 입력 상자에 URL을 입력: GET
    - 하이퍼링크를 클릭한 경우: GET
    - Form 사용의 경우: GET or POST

  - 메서드 입력 후 한 칸 띄고 URI 작성

  - 두 번째 행부터는 메시지 헤더 작성
  - 메시지 헤더 이후 공백 행 삽입, 이후 송신할 데이터 작성 (GET의 경우는 헤더에서 종료, POST는 Form 데이터 등)

- Response Message: Status Line + Message Header + Main Message
  - Status Code의 종류
    - 1xx: 처리 경과 상황 통지
    - 2xx: 정상 종료
    - 3xx: 다른 조치가 필요
    - 4xx: 클라이언트측의 오류
    - 5xx: 서버측의 오류
  - 태그 제어 정보의 경우, 서버에 해당 태그에 대한 Request Message를 추가로 발신하게 됨
    - ex) 영상 정보가 3개 포함된 문장을 읽을 경우 총 4회의 Request가 필요

### 2) IP Address

#### (1) IP 주소의 기본 (TCP/IP의 개념)

- 서브넷이라는 작은 단위 네트워크에 라우터가 접속

- 서브넷에 네트워크 번호를 할당, 이후 라우터에 호스트 번호를 할당

- IP Address = Network Number + Host Number

- IP 주소를 통해 엑세스 대상의 위치 특정, 데이터를 운반

- 메시지 발송 > 허브가 운반 > 라우터에서 다음 라우터를 판단 > 서버까지 같은 방식으로 발송

- IP 주소는 32비트의 디지털 데이터로 구성

- 네트워크 번호와 호스트 번호의 구분이 모호하므로 넷마스크를 사용, 내역을 결정

  ```text
  넷마스크가 1인 부분은 네트워크 번호, 0인 부분은 호스트 번호
  호스트 번호의 비트값이 모두 0인 경우 각 기기를 나타내는 것이 아닌 서브넷 자체를 지정
  호스트	번호의 비트값이 모두 1인 경우 서브넷 내의 모든 기기에 패킷을 발송하는 브로드캐스트 (@everyone)
  ```

#### (2) 도메인명과 IP 주소를 구분하여 사용하는 이유

- IP 주소를 써도 올바르게 작동하지만 서버의 이름을 기억하는 편이 훨씬 쉬움

- 가상 호스트 기능을 사용한 웹 서버의 경우 IP 주소로 엑세스가 불가능

- IP 주소 대신 상대를 이름으로 지정하여 통신하는 것은 효율이 좋지 않음

  - IP 주소는 4바이트로 충분, 도메인명은 최대 255바이트를 사용
  - 고성능 라우터를 사용하더라도 라우터 속도에 한계가 존재

- 사람은 도메인명을 사용, 라우터는 IP 주소를 사용

- 둘 중 하나를 알면 나머지 하나를 알 수 있다는 원리를 사용하여 양쪽의 차이를 해소 > DNS

  ```text
  DNS: Domain Name System, 주로 서버명과 IP 주소를 대응시키기 위한 시스템, 그 외의 기능도 보유
  ```

#### (3) Socket Library

- DNS 서버에 도메인명을 제공할 경우 IP 주소를 반환하는 것이 일반적인 IP 주소 조사법

- DNS Resolver: DNS 서버에 대응하는 클라이언트, Socket Library 내의 Resolver를 부품화한 프로그램

  ```text
  Library: 다양한 애플리케이션에서 이용할 수 있도록 부품화한 여러 개의 프로그램 모음집
  ```

- Name Resolution: DNS 원리를 사용하여 IP 주소를 조사하는 것

#### (4) DNS 서버 조회

- 애플리케이션 프로그램을 만들 때 Resolver 프로그램명과 웹 서버의 이름을 쓰면 Resolver 호출 가능
- DNS 서버에 조회 메시지를 보내고 응답 메시지를 받아 메시지 내 IP 주소를 추출하여 Browser에서 지정한 메모리영역에 입력
- OS에 송신을 의뢰할 때 메모리 영역에서 IP 주소를 추출해서 사용

#### (5) Resolver의 작동 방식

- Resolver 호출
  - 제어가 Resolver 내부로 넘어감
- Socket의 Resolver 내부 
  - DNS 서버에 보낼 조회 메시지 작성
  - OS 내부 Protocol Stack 을 통해 메시지를 보내고 응답 메시지를 수신
  - 응답 메시지에서 IP 주소를 추출, 메모리 영역에 저장
  - 애플리케이션으로 제어가 되돌아감

### 3) DNS Server

#### (1) DNS Server의 기본 동작

- 클라이언트로부터 조회 메시지를 받고 이에 대해 정보를 회답

  - Name: 서버 or 메일 배송의 목적지

  - Class: 인터넷 외의 네트워크를 고려한 정보, 현재 인터넷을 나타내는 "IN" 만 사용

  - Type: Name에 어떤 정보가 지원되는지를 나타냄, 타입에 따라서 회답 정보가 달라짐

    ```text
    A: 이름에 IP 주소가 지원
    MX: 이름에 메일 배송 목적지가 지원
    
    PTR, CNAME, NS, SOA 등의 Type이 더 있음
    ```

- Resource Record = Name, Class, Type 로 구성된 1건의 등록 정보

#### (2) Domain's Level

- 1대의 DNS 서버에 모든 정보를 등록하는 것은 불가능 > 다수의 서버에 정보를 분산시켜 등록

- DNS 서버에 등록한 정보에 계층적 구조를 가진 이름을 붙임 = "Domain Name"

  - "." 으로 Domain Level을 구분
  - 오른쪽으로 갈 수록 상위의 계층

  ```text
  www.lab.cyber.co.kr => kr 이하 co 이하 cyber 이하 lab의 www 로 볼 수 있음
  ```

  - "." 으로 구분된 각각의 계층 = "Domain"
  - 하위 계층은 상위 계층의 Sub Domain이라고 할 수 있음

#### (3) DNS 서버 탐색

- 하위 도메인 서버를 담당하는 DNS 서버의 IP 주소를 상위 도메인 서버에 등록
- 상위 도메인 서버에서 하위 도메인 서버의 IP 주소를 조회할 수 있으므로 조회 메시지를 보낼 수 있음
- 인터넷에 존재하는 모든 DNS 서버에 Root Domain DNS Server를 등록, 모든 서버에서 엑세스할 수 있음
- Root Domain을 경유하여 목표에 도달할 때까지 하위 도메인을 탐색, 원하는 DNS 서버에 도달하는 것이 가능

#### (4) Cache

- Cache: 한 번 사용한 데이터를 데이터의 이용 장소와 가까운 곳의 고속 기억 장치에 저장, 이후의 이용을 고속화하는 기술
- DNS 서버는 이 캐시 기능을 도입하여 한 번 조사한 이름을 기록, 이후 정보 요청시 빠르게 회답할 수 있음
- Cache 정보 != DNS 등록 정보, 변경될 수 있기 때문에 데이터의 유효 기간이 지나면 캐시에서 삭제 + 회답 시의 출처를 제공

### 4) Protocol Stack

#### (1) 데이터 송수신 개요

- IP  주소를 획득한 후, OS 내의 Protocol Stack에 엑세스 대상 웹서버로의 메시지 송신을 의뢰
- 브라우저뿐만이 아닌 모든 네트워크 애플리케이션에 해당
- Socket Library를 활용하며 Resolver 뿐만이 아닌 다양한 프로그램 부품을 사용
- Protocol Stack의 데이터 송수신 동작 4단계 요약
  - 소켓 생성 (소켓 작성)
  - 소켓에 파이프 연결 (접속)
  - 데이터 송수신 (송수신)
  - 파이프 분리 및 소켓 말소 (연결 끊기)
- HTTP 프로토콜은 각각의 데이터를 별도로 취급하여 해당 단계를 여러 번 반복하도록 만들어져 있음
- 복수의 Request와 송수신 실행하는 방법이 이후 만들어졌음

#### (2) 소켓 작성 단계

- Socket Library - socket 프로그램 호출
- socket 내부로 제어가 넘어가고 소켓 작성 동작 실행
- 작성 종료 후 제어가 돌아오면서 Descriptor를 반환
- Descriptor를 통해 각각의 socket을 식별할 수 있음

#### (3) 접속 단계 

- 작성한 socket을 서버 측 socket에 접속하도록 Protocol Stack에 의뢰

- Socket Library - connect 프로그램 호출

- Descriptor, Server IP Address, Port Number를 지정

  ``` text
  Port Number는 미리 결정된 값을 사용한다는 규칙이 존재
  ex) Web: 80, Mail: 25 등
  ```

- 클라이언트는 소켓의 Port Number를 적당한 값을 골라서 미리 할당, 접속 시 서버측에 통지

#### (4) 송수신 단계

- 송신

  - Socket Library - write 프로그램 호출

  - 애플리케이션이 송신 데이터를 메모리에 준비

  - write 프로그램 호출할 때 Descriptor, 송신 데이터를 지정

  - Descriptor로 지정한 소켓을 통해 연결된 상대에게 데이터를 송신

  - 네트워크를 통해 송신된 메시지를 서버가 수신, 내용 조사 및 처리 후 응답 메시지 반송

- 수신

  - Socket Library - read 프로그램 호출

  - 수신 버퍼 지정

    ```text
    수신 버퍼: 수신 메시지의 저장을 위한 메모리 영역
    ```

  - 버퍼에 저장된 메시지를 애플리케이션에 전달

#### (5) 연결 끊기 단계

- Socket Library - close 프로그램 호출
- 파이프 분리 및 소켓 말소
- 일반적으로 서버측에서 먼저 close를 호출하여 연결 제거
- 연결 제거가 클라이언트측에 전달되면 클라이언트 소켓이 연결 끊기 단계에 돌입
- Browser가 read를 통해 수신한 데이터를 호출한 경우 연결이 끊겼다는 사실을 통지
- 이후 Browser도 close를 호출하여 연결 끊기 단계에 돌입
