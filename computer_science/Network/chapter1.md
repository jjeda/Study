## 웹 브라우저가 메시지를 만든다

#### 1. HTTP 리퀘스트 메시지를 작성한다.
- HTTP: 무엇을? 어떻게?

#### 2. 웹 서버의 IP 주소를 DNS 서버에 조회한다.
- 브라우저는 HTTP 메시지를 만들지만 네트워크에 송출하는 기능은 없다. -> OS에 의뢰
- 이때 도메인명을 그대로 의뢰하는 것이 아닌 IPv4로 주어야함
- Socket의 리졸버를 통해 DNS서버를 조회한다.

#### 3. 전 세계의 DNS 서버가 연대한다.
- DNS 쿼리과정

#### 4. 프로토콜 스택에 메시지 송신을 의뢰한다.
- OS 내부 프로토콜 스택에서 
  - 1.소켓을만들고 2. 서버측의 소켓에 파이프를 연결하고 3.데이터를 주고받고 4.파이프 분리,소켓말소
- 브라우저에서 파이프를 연결하는 것이 아니다 OS가 하는것
- 하나의 브라우저에 여러개의 소켓이 있을 수있으므로 이를 식별할 수 있는 디스크립터가 필요함
  - 소켓이 생기면 이 디스크립터가 리턴됨
- 접속할 때는 1.디스크립터 2.IP주소 3.포트번호 이 3가지를 통해 접속함(2번단계)
  - 디스크립터와 포트번호를 구분해서 기억할 것
- 원칙은 하나의 요청당 한번의 소켓통신이 일어남 1~4번 과정이 반복됨
- 이 연결을 지속하는 방법들이 고안되었지~      