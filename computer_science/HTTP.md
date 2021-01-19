## HTTP
Contents 를 주고받기 위한 공통(server, client)의 약속
- [그림으로 배우는 HTTP & Network](https://book.naver.com/bookdb/book_detail.nhn?bid=8657832)



## HTTP 1.0 vs HTTP 1.1
- Keep-Alive: 커넥션을 재사용하여 시간을 절약
- Pipelining: 파이프라이닝 추가하여 응답이 완전히 전송되기 이전에 두번째 요청 전송을 가능케 하여 레이턴시를 낮췄다.
- Chunk Response: 청크된 응답 지원
- Cache control: 추가적인 캐시 제어 메커니즘이 도입
- Content Type: 언어, 인코딩 혹은 타입을 포함한 컨텐츠 협상이 도입되어, 교환하려는 가장 적합한 컨텐츠에 대한 동의 가능
- Host Header: 동일 IP 주소에 다른 도메인을 호스트하는 기능이 서버 코로케이션을 가능



## HTTP 1.1 vs HTTP 2.0
HTTP 메서드, 상태 코드, URI 및 헤더 필드는 그대로 유지
- Multiplexed Streams: 기존에는 요청 당 하나의 커넥션으로 Head Of Line blocking
  - 병렬 요청이 동일한 TCP 커넥션상에서 다루어지고, 서버 내에서 비동기적으로 웹파일을 다운로드, 순서가 없다.
- Stream Prioritization: 데이터에 의존관계를 설정, 우선순위를 부여할 수 있음
- Header Compression: 연속된 요청 사이의 유사한 내용으로 존재하는 헤더들을 압축
- Server Push: 서버로 하여금 필요하게 될 데이터로 채워넣음  
- Binary Protocol:텍스트 프로토콜이아닌 이진 프로토콜



## HTTP vs HTTPS

평문 통신 문제: 도청가능
- 통신 암호화: 컨텐츠를 암호화하지 않고 통신자체를 암호화하는 방법
- SSL, TLS 프로토콜과 HTTP를 조합
- SSL, TLS 로 안전한 통신로를 확립하고 여기에서 HTTP 통신하는 방

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


### 참고
- [google developers](https://developers.google.com/web/fundamentals/performance/http2/?hl=ko)