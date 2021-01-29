## 네트워크
참고도서
- [1%의 네트워크 원리](https://book.naver.com/bookdb/book_detail.nhn?bid=16386986)
- [그림으로 배우는 HTTP & Network](https://book.naver.com/bookdb/book_detail.nhn?bid=8657832)



## HTTP
Contents 를 주고받기 위한 공통(server, client)의 약속


**HTTP 1.0 vs HTTP 1.1**
- Keep-Alive: 커넥션을 재사용하여 시간을 절약
- Pipelining: 파이프라이닝 추가하여 응답이 완전히 전송되기 이전에 두번째 요청 전송을 가능케 하여 레이턴시를 낮췄다.
- Chunk Response: 청크된 응답 지원
- Cache control: 추가적인 캐시 제어 메커니즘이 도입
- Content Type: 언어, 인코딩 혹은 타입을 포함한 컨텐츠 협상이 도입되어, 교환하려는 가장 적합한 컨텐츠에 대한 동의 가능
- Host Header: 동일 IP 주소에 다른 도메인을 호스트하는 기능이 서버 코로케이션을 가능


**HTTP 1.1 vs HTTP 2.0**
HTTP 메서드, 상태 코드, URI 및 헤더 필드는 그대로 유지
- Multiplexed Streams: 기존에는 요청 당 하나의 커넥션으로 Head Of Line blocking
  - 병렬 요청이 동일한 TCP 커넥션상에서 다루어지고, 서버 내에서 비동기적으로 웹파일을 다운로드, 순서가 없다.
- Stream Prioritization: 데이터에 의존관계를 설정, 우선순위를 부여할 수 있음
- Header Compression: 연속된 요청 사이의 유사한 내용으로 존재하는 헤더들을 압축
- Server Push: 서버로 하여금 필요하게 될 데이터로 채워넣음  
- Binary Protocol:텍스트 프로토콜이아닌 이진 프로토콜


**HTTP vs HTTPS**
평문 통신 문제: 도청가능
- 통신 암호화: 컨텐츠를 암호화하지 않고 통신자체를 암호화하는 방법
- SSL, TLS 프로토콜과 HTTP를 조합
- SSL, TLS 로 안전한 통신로를 확립하고 여기에서 HTTP 통신하는 방법

통신 상대를 확인하지 않는 문제
- 위장한 웹서버 or 클라이언트
- 통신하고 있는 상대가 접근이 허가된 상대인지 아닌지 확인 불가능(IP, 포트 등의 제한이 없는경우)
- 어디의 누가 리퀘스트를 했는지 확인할 수 없음
- 의미없는 리퀘스트라도 수신하게 된다 (DDOS)
- SSL로 상대를 확인 할 수 있다 (증명서)

완전성을 증명할 수 없는 문제: 정보가 정확한지 아닌지 확인할 수 없음
- 발신된 후 수신하기 전에 변조되었다고 하더라도 사실을 알 수가없음(중간자 공격)
- MD5, SHA-1 등의 해시 값을 확인하는 방법, 디지털 서명 등을 확인하여 해결
- 이 해시값도 암호화방법에 맞게 수정된다면 유저로서는 알 수가없다
- SSL에는 인증, 암호화, 다이제스트 기능을 제공

HTTPS(206 ~ 207)
- 소켓 부분을 SSL, TLS 로 대체
- (private key - public key) 개념
- 공개키도 중간에 변경될 위험이 있음
- 인증 기관에서 발행하는 공개키 증명서가 이용되고있음
- 통신속도가 떨어진다는 단점
- CPU, 메모리 등을 소비하여 처리속도가 느려짐


## more
- 이진 프로토콜의 장점
- Session Layer, Presentation Layer
- 인증 
- HTTP 3.0


### Flow
- HTTP 1.0, 1.1, 2, 3, secure 는 어떤 문제를 해결하고자 했는가?



## URL 을 입력한 후 웹 페이지가 표시되기까지의 원리

**1. Client**

1. HTTP 리퀘스트 메시지를 작성
  - URI 분
  - 무엇을(URI) 어떻게(method)?
2. 웹 서버의 IP 주소를 DNS 서버에서 받아온다
  - 브라우저는 HTTP 메시지를 만들지만 네트워크는 운영체제에게
 

  
**2. Client -> Server gateway(reverser proxy)**
3. protocol stack
  - 소켓을 만들고, 서버측의 소켓에 파이프 연결, 데이터 주고받고, 파이프 분리, 소켓 말소
  - 디스크립터
  - l4 ~ l2 -> l2 ~ l4 decapsulation -> encapsulation 과정
  - ARP, IP, TCP
  - HTTPS의 경우 TLS 추
4. 방화벽
  - L3 헤더를 통해 IP 레벨에서 필터링, L4 헤더를 통해 포트 레벨에서 필터링
  - TCP 는 양뱡향이므로 한쪽을 정지시키면 멈춤(CTL bit)
5. 로드밸런싱
  - 복잡한 액세스의 경우 연결을 유지해야하는 이유(쿠키)
6. 캐싱
  - 포워드 프록스, 리버스 프록시

**3. Server**
7. server protocol stack
  - L3 에서 fragmentation 인지 판단
  - L4 에서 3-way handshaking 인지 판단(SYN bit)
  - 데이터를 주고받는 경우 수신 버퍼에 저장 후 애플리케이션 레벨에 전달
  - 소켓, 포트번호, 디스크립터 상호작용(dest, src ip/port 모두 필요한 이유)
8. 전통적인 서버(CGI), Servlet, netty, vertex 
9. Content-type, Content-Encoding
10. 다시 encapsulation ~ decapsulation 과정
11. 브라우저에 도착
  - DOM 트리
  - CSS parser
  - Render tree 기반으로 각 노드들의 크기 결정
  - 렌더링 엔진이 배치
