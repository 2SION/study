# 인터넷 네트워크
## 인터넷 통신
클라이언트 <-( 인터넷 )-> 서버
- 인터넷망은 수많은 노드들로 구성되어 있다.
- 그렇다면 어떤 규칙으로 어떻게 목적지까지 전달할 수 있을까?
## IP
* 인터넷 프로토콜
- 어떤 역할을 할까?
지정한 IP 주소에 데이터 전달
패킷이라는 통신 단위로 데이터 전달
* 클라이언트와 서버에 IP 주소를 부여한다.
클라이언트 : 100.100.100.1
서버 : 200.200.200.2
* IP 프로토콜의 한계
비연결성 : 패킷 수신 대상이 없거나 서비스 불능에도 패킷 전송
비신뢰성 : 중간에 패킷 손실, 패킷이 순서대로 오는가?
프로그램구분 : 같은 IP 사용하는 서버에서 애플리케이션이 여러개라면?
## TCP, UDP
프로토콜 계층
1. 프로그램이 Hello, world! 메시지 생성
2. SOCKET 라이브러리를 통해 전달
3. TCP 정보 생성, 메시지 데이터 포함
4. IP 패킷 생성, TCP 데이터 포함
[ IP 패킷 [ TCP 세그먼트 [ 전송데이터 ] ] ]
- IP 패킷 : 출발지 IP, 목적지 IP, 기타 등등
- TCP 세그먼트 : 출발지 PORT, 목적지 PORT, 전송 제어, 순서, 검증 정보 등등
### TCP 특징
- 연결지향 (3 way handshake)
- 데이터 전달 보증
- 순서 보장
- 신뢰할 수 있는 프로토콜
- 대부분 TCP 사용
### UDP 특징
- 연결지향 (3 way handshake) X
- 데이터 전달 보증 X
- 순서 보장 X
- 데이터 전달 및 순서가 보장되지 않지만, 단순하고 빠르다.
=> IP에서 PORT와 체크섬 정도만 추가
=> 애플리케이션에서 추가 작업이 필요하다.
## PORT
같은 IP 내에서 프로세스 구분하는 역할
IP는 아파트, PORT는 동/호수라고 생각하면 된다.
## DNS
IP는 기억하기 어렵다. 그리고 IP는 변경될 수 있다.
Domain Name System
- 전화번호부 역할
- 도메인 명을 IP 주소로 변환


# URI와 웹 브라우저 요청 흐름
## URI
URI(Uniform Resource Identifier)는 로케이터(Locater), 이름(Name) 또는 둘다 추가로 분류될 수 있다.(위치는 변할 수 있지만, 이름은 변하지 않는다.)

[ URI > URL&URN ]

- Uniform 리소스 식별 통일 방식
- Resource URI로 식별할 수 있는 모든 자원
- Identifier 다른 항목과 구분하는데 필요한 정보

## URL
전체문법
- 프로토콜(https)
  * 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙(http, https, ftp)
  * http는 80, https 443 포트 사용
- 호스트명(www.google.com)
  * 도메인명이나 아이피주소
- 포트번호(443)
  * 생략 가능
- 패스(/search)
  * 리소스 경로, 계층적 구조(디렉토리처럼)
- 쿼리 파라미터(q=hello&hi=ko)
  * key=value 형태
  * ?로 시작, &로 추가
  * 쿼리 파라미터, 쿼리 스트링 등으로 불림. 웹서버에 제공하고 파라미터, 문자 형태

## 웹 브라우저 요청 흐름
구글서버 찾기
1. DNS 서버 조회(IP와 포트 정보 찾기)
2. http 요청 메시지 생성

HTTP 메시지 전송
1. 웹브라우저가 HTTP 메시지 생성(메소드 + 쿼리 + HTTP 버전 정보)
2. socket 라이브러리를 통해 전달
  * TCP/IP 연결(IP,PORT)
  * 데이터 전달
3. TCP/IP 패킷 생성, HTTP메시지 포함
4. 요청 패킷 전달 -> 구글 서버에 도착
5. 구글 서버에서 HTTP 응답 메시지 생성
  ```
  HTTP/1.1 200 OK
  Content-Type: text/html;charset=UTF-8
  Content-Length:3423
    
  <html>
    <body>...</body>
  </html>
  ```
6. 응답 패킷 전달 -> 웹 브라우저에 도착

# HTTP 
## 기본
* HyperText Transfer Protocol
* HTTP 메시지에 모든 것을 전송
  - HTML, TEXT
  - IMAGE, 음성, 영상, 파일
  - JSON, XML (API)
  - 거의 모든 형태의 데이터들을 전송 가능
  - 서버간에 데이터 주고 받을 때에도 HTTP 사용
* HTTP 버전
  1. HTTP/1.1
    - 1997년 : 가장 많이 사용, 우리에게 중요한 버전
  2. HTTP/2 2015년 : 성능개선  (HTTP/1~2 TCP프로토콜)
  3. HTTP/3 진행중 : TCP 대신 UDP 사용, 성능 개선
* HTTP 특징
  - 클라이언트 서버 구조
  - 무상태 프로토콜(스테이리스), 비연결성
  - HTTP 메시지
  - 단순함, 확장 가능

## 클라이언트 서버구조
클라이언트 -(1. 요청)→ ... ←(2. 응답)- 서버
  - Request Response 구조
  - 클라이언트가 서버에 요청을 보내고 응답 대기
  - 서버가 요청에 대한 결과를 만들어서 응답

## 무상태 프로토콜
Stateful, Stateless
점원을 통해 노트북 구매하는 과정으로 비유

### Stateless
서버가 클라이언트 상태를 보존하지 않는다.
장점 : 서버 확장성이 높음 (스케일 아웃)
단점 : 클라이언트가 추가 데이터 전송해야 한다.
**중간에 다른 점원으로 변경해도 된다.**
고객이 증가하더라도 점원 투입 가능
갑작지 클라이언트 요청이 증가해도 서버 투입 가능
=> 응답 서버를 쉽게 바꿀 수 있다. (서버 증설이 용이)
실무 한계
모든 부분을 무상태로 설계할 수 없는 경우도 있다.
무상태 : 로그인이 필요없는 단순한 서비스 소개 화면
상태 유지 : 로그인
로그인한 사용자의 경우 로그인 상태를 서버에 유지
일반적으로 브라우저 쿠키와 서버 세션등을 사용하여 상태 유지
### Stateful
서버가 클라이언트의 이전 상태를 보존한다.
중간에 다른 점원으로 변경하면 안 된다.
(상태 정보를 다른 점원에게 미리 알려줘야 한다.)

## 비 연결성
- HTTP는 기본이 연결을 유지하지 않는 모델
- 일반적으로 초 단위의 이하의 빠른 속도로 응답
- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
  * ex) 웹 브라우저에서 계속 연속으로 검색 버튼을 클릭하지 않는다.
- 서버 자원을 매유 효율적으로 사용할 수 있음
- 한계와 극복
  * TCP/IP 연결을 새로 맺어야 함 - 3 way handshake 시간 추가
  * 웹 브라우저로 사이트를 요청하면 HTML과 더불어 자바스크립트, css, 추가 이미지 등 수 많은 자원이 함께 다운로드
  * 지금은 HTTP 지속 연결(Persistent Connections)로 문제 해결
  * HTTP/2, HTTP/3에서 더 많은 최적화