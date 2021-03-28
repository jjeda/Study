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