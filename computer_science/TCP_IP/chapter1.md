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
