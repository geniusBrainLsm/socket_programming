# socket_programming
네트워크보안 시험정리

# 소켓프로그래밍이란?
네트워크 상에서 컴퓨터 간에 데이터를 주고받기 위해 사용되는 프로그래밍 기법
가장 일반적인 프로토콜은 TCP와 UDP




## 네트워크 주소에 대한 구조체(2)
sockaddin이라는 구조체에 대해 맴버들을 살펴보기. 맴버들에 대해서 각각에 대한 기능들
어떤 프로토콜을 쓸려면 어떤 값을 넣어야하는지? 에 대해서

```
  struct sockaddr_in {
      sa_family_t    sin_family;   // 주소 체계(Address Family)
      uint_16t       sin_port;     // 16비트 TCP/UDP 포트 번호
      struct in_addr sin_addr;     // 32비트 IPv4 인터넷 주소
      char           sin_zero[8];  // 사용되지 않으며, 0으로 채워져야 함
  };
```

인데, 
```
  int main(void) {
    struct sockaddr_in server_addr;

    memset(&server_addr, 0, sizeof(server_addr)); // 구조체 초기화
    server_addr.sin_family = AF_INET; // 주소 체계 설정
    server_addr.sin_port = htons(atoi(argv[2])); // 포트 번호 설정
    server_addr.sin_addr.s_addr = inet_addr(argv[1]); // IP 주소 설정

    // 이후 socket, bind, listen, accept 등의 함수를 사용하여
    // 소켓을 열고 클라이언트의 연결을 기다릴 수 있습니다.
    
    return 0;
}
```
이런식으로 쓰는거임. 
>sin_family: 주소 체계를 나타냅니다. 대부분 AF_INET을 사용합니다.
>
>sin_port: 16비트 포트번호를 나타냅니다. htons()함수를 사용하여 네트워크 바이트 순서로 변환해야 합니다.
>
>sin_addr: 32비트의 IPv4 주소를 나타냅니다. inet_addr() 함수를 사용하여 변환할 수 있습니다.
>
>sin_zero: 아무런 기능을 하지 않는 8바이트 필드로, 0으로 채워져야 합니다. 이 필드는 sockaddr와 sockaddr_in 구조체의 크기를 동일하게 맞추기 위해 사용됩니다.
>
>sockaddr_in 구조체는 주로 소켓 함수의 인자로 사용됩니다. 예를 들어, bind(), connect(), sendto(), recvfrom() 등의 함수에서 사용됩니다. 이를 통해 소켓에 IP 주소와 포트 번호를 할당하거나, 원격 호스트와의 연결을 설정할 수 있습니다.











## 주소 변환에 관련된 함수들
>inet_addr(): Dotted Demical Notation을 Big-Endian 32비트 정수형 데이터로 변환. 인자는 ip형태의 주소값(15), 결과값은 4바이트 정수

>inet_network(): Dotted Demical Notation을 호스트 데이터 형태 32비트 정수형 데이터로 변환. htonl()함수 호출 필요. 인자는 ip형태의 주소값(15), 결과값은 4바이트 정수

>inet_aton(): addr이랑 비슷함. 근데 두번째 인자가 출력물임. (4바이트 정수)

>inet_ntoa(): addr이랑 반대임. Re-entrance문제 발생가능.(다중 스레드 사용시) 인자값은 같음. 결과값이 문자열 주소(DDN)











## 소켓 프로그래밍을 할려면 소켓을 만드는데, 그 안에 어떤 프로토콜을 쓸거다 라고 지정하기 위해서 명시하는 그런 내용에 대해서
(어떤프로토콜을 사용하기 위해 소켓을 쓸려고한다, 적절하게 소켓함수를 작성하시오)

>TCP 소켓 생성
socket()을 사용하고, IPv4를 쓰겠다는 의미인 AF_INET을 지정
type 항목에는 SOCK_STREAM을 지정
>int tcp_socket = socket(AF_INET, SOCK_STREAM, 0);

>int udp_socket = socket(AF_INET, SOCK_DGRAM, 0);(상동)











## 바인딩 관련사항. 서버, 클라이언트카 각각 언제 바인드가 되는지하는지, TCP UPD에 따라서 바인딩 시점이 다르다. 시점?

강의자료에 별표로 표시한것.. TCP부분에 있다. 클라이언트에서는 어떤함수의 순차로, 서버에서는 어떤함수의 순차로 호출되는지 호출순서
![h464h5](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/9237ba2d-9db6-43c0-a353-cea2556605df)
![23f](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/6bb4ca7e-5ee1-4435-aa37-94a3eec01086)













## 실행을 편하게 하는 디버거 netcat (nc)명령어 어떤 조건을 명시하면, 그에 맞는 nc 명령어를 옵션을 조합해서 
nc {-options} [hostname] [port]
>- -u : UDP 모드 동작(생략이면 TCP 모드로 동작함)
>- 
>– -t : telnet형태로 동작(TCP)

>– -l : listen 모드 동작(서버로 사용 시)
>
>– -p : localhost의 포트번호(서버로 사용 시)









## TCP로 데이터를 송수신할떄 쓰는 함수, 그함수 필드로써 어떤 값이 명시가 되는지에 따라 기능을 달리할수있는데 그 플래그값
수신함수: recv() 
>  파일기술자를 통해 지정한 개수만큼 데이터를 읽음
> 
>  파일기술자가 소켓 타입이면 수신버퍼에서 데이터를 읽어 들임
> 
>  소켓을 통해 수신된 데이터를 지정한 개수만큼 읽어들임.

  인자값: 
  >fd: 소켓의 파일기술자
>
  >*buf: 수신된 데이터가 기록될 공간
>
  >count:수신가능 길이(버퍼의 크기)
>
  >flags:운영방식
>
  결과값: 성공: 수신 데이터 길이
          실패:-1
  ```
    ssize_t recv(int sockfd, void *buf, size_t len, int flags);
  ```
  플래그값: 
  >MSG_DONTWAIT : 수신 버퍼에 데이터가 없으면 종료
>
  >MSG_OOB      :Out of Bound 데이터를 수신
>
  >MSG_PEEK     :수신 버퍼에서 데이터를 읽을 때, 수신 버퍼의 내용을 그대로 둠.
>
  >MSG_WAITALL  :지정한 개수까지 수신버퍼에서 읽어들일때까지 대기(Blocking)

송신함수: send() :
   파일기술자를 통해 지정한 개수만큼 데이터를 송신
   파일기술자가 소켓 타입이면 송신 버퍼에 데이터를 기록
   연결형 소켓을 통해 전달될 송신 버퍼에 데이터를 지정한 개수만큼 기록

   인자값:recv랑 같음
   결과값: 송신버퍼에 기록한 데이터의 길이. 실패시 -1

  플래그값: 
  >MSG_DONTROUTE : 동일 네트워크에 있는 호스트에게만 전달
>
 >MSG_DONTWAIT  : 송신 버퍼에 기록을 못하는 상태이더라도 대기하지않음.
>
 >MSG_OOB      :Out of Bound 데이터를 송신













## 4개의 문제가 잇는데, 그 4개는 멀티플랙싱 관련된 문제로 이루어짐. 멀티플랙스 함수 여러개가 있는데 그거 알아둘것
![pol;l](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/8dbe6b54-87f1-43eb-b01e-fde17fb8b8c6)

![poll 인자](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/a4eec2c8-4265-40e9-b357-a0be60efa85c)

![pollin구조체](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/5749fae2-4a0c-4e15-808f-0e9cec823a97)
![poll 예](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/db4347ed-13b6-41d5-8370-3c45c831d6fe)



![select](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/6cf304cf-e54f-4d04-a826-dfb5d59451df)
![select관련함수](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/f9f117df-c44e-44e8-9bb6-52dfb9045c43)

![스크린샷 2023-12-12 오전 10 06 52](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/1c04b99e-3072-4028-b08c-c9e836381c04)
![스크린샷 2023-12-12 오전 10 10 51](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/3bf0df6f-59a4-4db7-81d0-1fabe0b8b1b0)








여기까지가 17번까지 정리고, 18번은 교수님 성함ㅋㅋ

나머지는 코드 중 빈칸채우기.
## 멀티플랙스 기법을 쓴 코드에 빈칸을 채우기. 멀티플랙스 기반의 소스를 공부. 참고로 이 코드는 UPD기반임..
UDP기반 멀티플랙싱 코드다 하면 어떤 내용을 채워야 할지
![udp클라이](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/22f2b9ed-75be-42c6-8b53-42cb63726b77)

모니터링 대상과 타임아웃이 동일하다면 검사할 FD와 타임아웃에 대해 재설정 필요가 없다.

![select](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/b6fd9893-542b-48d4-94a1-ffa01d48ffe5)

![poll](https://github.com/geniusBrainLsm/socket_programming/assets/87559232/616893e6-abcd-47f8-8bfc-ade0542506da)

