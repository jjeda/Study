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