## chapter 1: 네트워크 프로그래밍과 소켓의 이해

### Network Programming
- 소켓 기반의 프로그래밍
- 네트워크로 연결된 컴퓨터 사이의 데이터 송수신 프로그램 작성


### server 소켓(listening socket)
- 네트워크의 연결도구
- OS 가 제공하는 Software
- 데이터 송수신에 대한 물리적, 소프트웨어적 세세한 내용을 신경 쓰지마

(TCP 소켓)
소켓 생성 : 핸드폰 구입
주소 할당 : 전화번호 개통

소켓생성: socket 함수 호출
수신과 송신할 때 소켓 생성 방법 차이가 있음

소켓생성은 전화기를 구입히는것과 같구나
 
```cpp
#include <sys/socket.h>
int socket(int domain, int type, int protocol);
```

고유한 번호(IP:Port)가 있어야 구분이 되겠구나
bind 함수를 호출해서 번호를 할당받자(주소 할당)

```cpp
#include <sys/socket.h>
int bind(int sockfd, struct sockaddr *myaddr, socklen_t addrlen);
```

전화기를 전화망에 연결하여 전화를 받을 수 있는 상태로하자
이건 전화를 받는 소켓

```cpp
#include <sys/socket.h>
int listen(int sockfd, int backlog);
```

누군가 전화를 걸었을 때 수화기를 들어야함 가만히있으면 연결되는거 노노
전화기를 들어서 받았으면 송수신은 양방향으로 가능함
```cpp
#include <sys/socket.h>
int accept(int sockfd, struct sockaddr *addr, soclen_t *addrlen);
```

소켓 생성 -> 번호 할당 -> 전화오기를 기다리는 중 -> 누군가한테 전화가오면 받는다


### client 소켓

전화기에 비유하기가 힘듦
소켓 생성 -> 주소 할당 받고 뭐 이런 과정들이 자동화되어있음

```cpp
#include <sys/socket.h>
int connect(int sockfd, strucy sockaddr *serv_addr, socklen_t addlen);
```


## chapter 2: 소켓의 타입과 프로토콜의 설정

### Protocol
- domain: 데이터를 전달 할 도구 (편지)
  - IPv4, IPv6...
- type: (일반, 빠른) 편지 || 등기
  - 소켓이 생성될 때 소켓의 타입도 결정되어야 한다
  - 연결 지향형 소켓 타입, 비 연결 지향형 소켓 타입
- protocol: 일반 or 빠른
  - 등기를 선택했을 때 protocol 이 하나만 있다면 타입을 선택하면 자동으로 선택이된다.

### type
- 연결지향형 소켓(SOCK_STREAM)
  - 데이터 보존
  - 순서 보존
  - 데이터의 경계가 존재하지 않는다: 한 번의 read 로 여러개의 write 를 읽을 수 있음
  - 1:1 의 구조(전화)
- 비 연결지향형 소켓(SOCK_DGRAM)
  - 상자에 데이터를 담아 빠르게 보낼건데 중간에 데이터 손실되어도 나는 몰라
  - 데이터, 순서 보존 x but 빠름
  - 데이터 경계 존재: write 3byte 했으면 read 도 3byte (상자 하나 주고받음)
  - 데이터 크기 제한(상자크기)
  
```cpp
int tcp_socket = socket(PF_INET, SOCK_STREAM, IPPROTO_TCP);
int udp_socket = socket(PF_INET, SOCK_DGRAM, IPPROTO_UDP);
```



## chapter 3: 주소체계와 데이터 정렬

### PORT
- 소켓을 구분하는 용도
- 둘 이상의 PORT 가 하나의 프로그램에 할당될 수 있음

### sockaddr_in
- sin_family: 주소체계
- sin_port: PORT 번호
- sin_addr: 32비트 IP 주소
- sin_zero: 크기 채우기용

### 소켓에 주소 할당
```cpp
#include <sys/socket.h>
int bind(int sockfd, struct sockaddr *myaddr, socklen_t addrlen);
```
- sockfd: 주소 정보를 할당할 소켓의 디스크립터
- myaddr 할당하고자 하는 주소정보



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



## chapter 5: TCP 기반 서버 / 클라이언트 2

### 에코 클라이언트의 문제점
- 서버는 데이터의 경계를 구분하지 않고, 수신된 데이터를 그대로 전송할 의무만 있음
- 클라이언트는 문장 단위로 데이터를 송수신(경계를 구분해야함)
  - write(sock, message, strlen(message));
  - str_len=read(sock, message, BUF_SIZE-1);
  - 문제가 생김..

```cpp
// while 을 통해 해결
str_len=read(sock, message, BUF_SIZE-1);

recv_len = 0
while(recv_len < str_len)
{
    recv_cnt = read(sock, &message[recv_len], BUF_SIZE-1);
    if (recv_cnt == - 1) error_handling("read() error!");
    recv_len += recv_cnt;
}
message[recv_len] = 0;
```

### TCP 소켓 입출력 버퍼
- TCP 소켓에 각각 별도로 존재
- 소켓생성시 자동으로 생성
- 소켓을 닫아도 출력버퍼에 남아있는 데이터는 계속 전송이 이뤄짐
- 소켓을 닫으면 입력버퍼 데이터는 소멸


## chapter 6: UDP 기반 서버/클라이언트

### UDP
- 기능이 거의 없다
- 따라서 커스터마이징이 필요한 경우에는 TCP가 아닌 UDP 를 선택
- 개발자가 어떻게 개발하냐에 따라서 TCP 의 기능을 모두 커버할 수 있는 것
- SEQ, ACK 같은 메시지를 전달하지 않음(Flow Control 없음)
- UDP 연결의 개념이 존재하지 않는다(서버, 클라이언트 소켓의 구분이 없음)
- 하나의 소켓으로 둘 이상의 영역과 데이터 송수신(1:1이 아님)


### TCP 송수신과의 차이점
- 연결의 개념이 없으므로 데이터를 전송할 때마다 목적지에대한 주소정보가 포함되어야함
- TCP read(), write() 에는 소켓에 대한 정보만 있었음
- UDP 에서는 데이터 경계가 존재해서 단순히 sendTo -> recvFrom 가 반복되어도 됨


### connected UDP 소켓
- 연결의 개념이 아님
- unconnected: UDP 소켓에 목적지에 대한 IP와 PORT 번호를 등록 -> 데이터 전송 -> 목적지 정보 삭제
- 매번 주소를 할당하고 해제하는게 성능이 좋지 않기때문에 목적지 정보만 유지하는 것
- 상대와의 연결을 유지하는 개념이 아닌 내가 혼자 목적지만 기록해두는 개념 



## chapter 7: 소켓의 우아한 종료

### 일방적인 연결 종료의 문제점
- 전화받는 상대방이 아직 할말이 남았는데 끊어버렸..
- 전화를 끊은지는 알 수 있음(EoF)
- 상대방이 보내준 데이터를 다 받았다는 확신을 못함

### Half-close
- 다 보냈으면 전송에대한 스트림만 종료하고 수신에 대한 스트림은 열어둬
- 전송 스트림을 종료(close())하면 상대방(EoF 수신)은 수신할 데이터가 없다는걸 알 수 있음
- 4-way handshake 랑은 구분하자
  - buffer 의 관점임(half-close 는 application 관점)
  - buffer에 아직 전송해야 할 데이터가 남아있을 수 있으니 남아있는 buffer 의 전송을 보장하기위해
  
  
## chapter 8: 도메인 이름과 인터넷 주소

### DNS
- 단순히 IP 주소 <> 도메인 이름을 변환하는 것이 아님
- IP 주소로 도메인의 모든 정보(도메인이름 포함) 가져오는 것
- 도메인 이름으로 해당 도메인의 모든 정보(IP 주소 포함) 가져온다(getHostByName())
- IP 는 상대적으로 변동이 심하기때문에, 도메인 이름을 이용해서 서버가 실행될 때 IP를 얻어오게 구현하자


## chapter 10: 멀티프로세스 기반의 서버구현
- 멀티 프로세스 기반: 다수의 프로세스를 생성하는 방식
- 멀티플렉싱 기반: 입출력 대상을 묶어서 관리하는 방식
- 멀티쓰레딩 기반: 클라이언트 수만큼 쓰레드를 생성하는 방식

### fork()
 - 같은 프로그램을 여러 개의 프로세스가 처리하자 라는 목적에는 fork() 함수만을 사용
 - 함수가 실행되면 실행한 프로세스(부모)와 함께 새로운 프로세스(자식)가 1개 생성
 
 1. 자식 메모리 영역 작성 후 부모의 메모리 복사
 2. fork() 함수의 리턴값이 다른 것을 이용하여 부모, 자식이 다른 코드를 실행 하도록 분기
   - 부모는 자식 프로세스의 pid, 자식은 0 리턴

### 좀비 프로세스
- 실행이 완료되었음에도 소멸되지 않은 프로세스
- 리소스가 메모리 공간에 여전지 존재함
- 반환하는 상태 값이 부모에게 전달되이 않았을 때
  - 인자를 전달하면서 exit
  - main 함수 return 문에서 값을 반환 

### 시그널
- 특정 상황이 되었을 때 운영체제가 프로세스에게 알림
- SIGALRM: alarm 호출
- SIGINT: CTRL + C
- SIGCHLD: 자식 프로세스 종료
