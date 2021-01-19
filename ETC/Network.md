## Network
URL 을 입력한 후 화면이 보일 때까지 어떤 일이 벌어지는가?

1. HTTP 리퀘스트 메시지를 작성
  - URI 분
  - 무엇을(URI) 어떻게(method)?
2. 웹 서버의 IP 주소를 DNS 서버에서 받아온다
  - 브라우저는 HTTP 메시지를 만들지만 네트워크는 운영체제에게
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



  
  
      
  
