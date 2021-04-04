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
