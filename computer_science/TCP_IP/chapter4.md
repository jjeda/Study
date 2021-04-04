## chapter 4: TCP 기반 서버 / 클라이언트

### TCP / IP 프로토콜 스택
- 송수신을 목적으로 설계된 스택
- Link: 물리영역의 표준
- IP: 경로 설정(라우팅)
- Transport: 실제 데이터 송수신과 관련
- Application  

### 함수호출
- socket() -> bind(): bind 함수 호출까지 호출이 되면 주소가 할당된 소켓을 얻게 됨
- listen(): 연결 요청이 가능한 상태가 되어야 함, 연결 요청을 받아들이기 위해서 하나의 소켓이 필요(서버소켓, 리스닝 소켓)
```cpp
#include <sys/type.h>
int listen(int sock, int backlog);
```
- accept: 연결 정보를 참조하여 클라이언트 소켓과의 통신을 위한 소켓을 추가로 하나 더 생성
