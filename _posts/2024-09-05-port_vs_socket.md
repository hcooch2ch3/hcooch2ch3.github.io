---
layout: post
title: Port vs Socket
tags:
  - Port
  - Socket
---

포트와 소켓은 자주 사용하는 개념인데 "나의 언어"로 설명할 수 없어서 공부할겸 한번 정리해보고자 합니다.

먼저 ChatGPT 한테 물어봐서 감을 잡고, 나머지 참고 자료 내용을 정리하겠습니다.

## GPT 답변

```markdown
# 포트 (Port)

포트의 역할과 기능
• 네트워크 통신에서의 포트: 포트는 TCP/IP 네트워크에서 특정 프로세스 또는 서비스에 대한 통신 엔드포인트를 나타냅니다. 네트워크에서 장치가 통신할 때 IP 주소는 장치 자체를 식별하고, 포트 번호는 해당 장치에서 실행 중인 특정 애플리케이션이나 서비스를 식별합니다.
• 포트 번호 범위:
• 잘 알려진 포트 (Well-known Ports, 0-1023): HTTP(80), HTTPS(443), FTP(21), SSH(22) 등과 같은 표준 프로토콜과 서비스에 할당된 포트입니다.
• 등록된 포트 (Registered Ports, 1024-49151): 특정 애플리케이션 및 서비스에 사용되는 포트 번호로, IANA(Internet Assigned Numbers Authority)에 등록되어 있습니다.
• 동적/사설 포트 (Dynamic/Private Ports, 49152-65535): 일시적으로 사용되는 포트 번호로, 주로 클라이언트가 서버와의 통신을 위해 임시로 할당받는 경우 사용됩니다.

포트의 사용 예

• 웹 브라우징: 사용자가 웹 브라우저를 통해 웹사이트에 접속할 때, 브라우저는 클라이언트로서 서버의 IP 주소와 포트 80(HTTP) 또는 443(HTTPS)으로 연결 요청을 보냅니다.
• 파일 전송: FTP 클라이언트가 서버에 파일을 전송하거나 받을 때, 서버의 포트 21을 통해 연결이 이루어집니다.

# 소켓 (Socket)

소켓의 정의와 역할

• 소켓의 정의: 소켓은 네트워크 상의 양쪽 끝을 나타내는 논리적인 엔티티로, 네트워크를 통해 데이터를 송수신하는 인터페이스입니다. 소켓은 IP 주소와 포트 번호로 구성되며, 이는 네트워크 통신에서 고유한 식별자를 제공합니다.
• 소켓의 기능: 소켓은 네트워크에서 데이터 전송을 위한 두 호스트 간의 논리적인 연결을 설정하고 유지하는 데 사용됩니다. 네트워크 애플리케이션은 소켓을 통해 데이터를 주고받을 수 있습니다.

소켓의 유형

1. 스트림 소켓 (Stream Socket):
   • 프로토콜: TCP(Transmission Control Protocol)를 사용.
   • 특징: 연결 지향적이며, 데이터의 신뢰성을 보장하고, 패킷의 순서가 보장됩니다.
   • 사용 예: 웹 서버와 클라이언트 간의 연결, 이메일 전송, 파일 전송 등.
2. 데이터그램 소켓 (Datagram Socket):
   • 프로토콜: UDP(User Datagram Protocol)를 사용.
   • 특징: 비연결 지향적이며, 데이터의 신뢰성을 보장하지 않고, 패킷의 순서를 보장하지 않습니다.
   • 사용 예: 실시간 스트리밍, 온라인 게임, VoIP 등.
3. 원시 소켓 (Raw Socket):
   • 프로토콜: IP 프로토콜을 직접 사용.
   • 특징: IP 패킷의 헤더 정보에 접근이 가능하며, 네트워크 프로토콜의 구현 및 분석에 주로 사용됩니다.
   • 사용 예: 네트워크 프로토콜 개발, 침입 탐지 시스템 등.

소켓의 동작 과정

1. 서버 소켓 생성: 서버는 특정 포트를 열어 수신 대기 상태로 소켓을 생성합니다.
2. 클라이언트 소켓 생성 및 연결 요청: 클라이언트는 자신의 IP와 포트를 사용해 소켓을 생성하고, 서버의 IP와 포트로 연결을 요청합니다.
3. 연결 수락 및 데이터 송수신: 서버는 클라이언트의 요청을 수락하여 연결을 확립하고, 이후 두 소켓 간에 데이터 송수신이 이루어집니다.
4. 연결 종료: 통신이 끝나면 양쪽 소켓은 연결을 종료하고, 리소스를 해제합니다.

포트와 소켓의 상호 작용

포트와 소켓은 네트워크 통신에서 서로 밀접하게 관련되어 있습니다. 포트는 특정 네트워크 애플리케이션을 식별하기 위한 논리적인 숫자이며, 소켓은 이 포트를 사용하여 실제 네트워크 통신을 수행하는 인터페이스입니다. 예를 들어, 웹 브라우저가 웹 서버에 접속할 때, 웹 서버는 포트 80(HTTP)을 열고 기다리고 있으며, 클라이언트 소켓은 서버의 IP 주소와 포트 80으로 연결 요청을 보냅니다. 이렇게 설정된 소켓을 통해 클라이언트와 서버는 데이터를 주고받을 수 있습니다.
```

GPT 답변을 정리하면, "**포트**는 네트워크에서 특정 애플리케이션을 식별하는 논리적 번호"이고, "**소켓**은 IP 주소와 포트 번호를 조합하여 실제 네트워크 통신을 수행하는 인터페이스"라고 정리할 수 있겠습니다.

## RFC 793

```markdown
- port: The portion of a socket that specifies which logical input or output channel of a process is associated with the data.

- socket: An address which specifically includes a port identifier, that is, the concatenation of an Internet Address with a TCP port.
```

RFC 793에 따르면 "**포트**는 소켓의 일부분으로 프로세스의 논리적 입출력 채널을 지정하고, **소켓**은 인터넷 주소와 TCP 포트가 결합된 주소이다." 라고 정의되어 있습니다. 

## Oracle Java Documentation

```markdown
"A socket is one endpoint of a two-way communication link between two programs running on the network. A socket is bound to a port number so that the TCP layer can identify the application that data is destined to be sent to."
```

Oracle Java 문서에 "**소켓**은 네트워크상의 두 프로그램 간 양방향 통신 링크의 한쪽 끝점이며, TCP 계층이 데이터를 보낼 애플리케이션을 식별할 수 있도록 포트 번호에 바인딩된다." 라고 합니다. 

## Computer Networking - A Top Down Approach, 7th,

```markdown
"a process (as part of a network application) can have one or more sockets, doors through which data passes from the network to the process and through which data passes from the process to the network."

"Because at any given time there can be more than one socket in the receiving host, each socket has a unique identifier."
```

이 책에선 "**소켓**은 프로세스가 네트워크와 데이터를 주고받는 문(door) 역할을 하며, 여러 소켓이 존재할 수 있기 때문에 각각 고유한 식별자를 가진다"라고 합니다.

## 정리

### 포트 (Port)
- 포트는 프로세스가 데이터를 주고받는 통로 번호라고 생각하면 됩니다.
- 한 컴퓨터 안에는 여러 프로그램이 동시에 네트워크를 사용할 수 있습니다.
   - 웹 브라우저는 HTTP(80번 포트)를 통해 서버와 통신
   - 이메일 클라이언트는 SMTP(25번 포트)를 통해 메일 전송
- 즉, "어떤 프로그램(프로세스)으로 데이터를 보내야 할지"를 식별하는 번호가 포트입니다.
- 비유하자면 건물(컴퓨터) 안에 여러 사무실(프로세스)이 있고, 각 사무실 문 앞에 붙어 있는 호실 번호(포트 번호) 같은 것이라고 비유할 수 있습니다.

### 소켓 (Socket)
- 소켓은 **IP 주소(컴퓨터의 주소) + 포트 번호**을 식별자로 하여, 애플리케이션이 TCP 또는 UDP 프로토콜을 이용하여 통신을 가능하게 하는 인터페이스입니다.
- 네트워크에서 통신할 때는 단순히 컴퓨터 IP만으로는 충분하지 않으므로, 컴퓨터 안의 어느 프로그램인지까지 알아야 하기 때문에 "IP:포트" 형태로 특정 통신 채널을 구분합니다.
- 그리고 소켓은 이렇게 구분된 통신 채널로 들어온 요청을 해당 애플리케이션 프로세스에 전달하고, 프로세스가 보낸 응답을 다시 네트워크를 통해 상대방에게 전송하는 역할을 합니다. 즉, 운영체제의 네트워크 계층과 애플리케이션 간을 이어주는 일종의 창구라고 할 수 있습니다.
- 더 나아가, 소켓은 네트워크의 복잡한 동작(TCP 연결 설정, 패킷 전송/수신, 오류 처리 등)을 애플리케이션이 직접 다루지 않도록 **“통신을 위한 추상화된 인터페이스”**를 제공합니다. 따라서 애플리케이션 개발자는 저수준 네트워크 프로토콜의 세부사항을 몰라도 소켓 API를 통해 손쉽게 데이터를 송수신할 수 있습니다.
- 마치 HTTP를 추상화한 REST API와도 유사하다고 볼 수 있을 것 같습니다. 웹 개발자는 REST API를 통해 HTTP 요청/응답 형식을 직접 다루지 않고 서버와 통신을 할 수 있기 때문입니다.
- 또한 비유하자면 어떤 회사에서 퀵배송 업무를 대리해서 전담하는 직원이 있다고 할때, 이 직원에게 "건물 주소(IP) + 호실 번호(포트)를 합친 완전한 주소(소켓)"로의 퀵 송부 업무를 요청하면 나머지 작업은 이 직원이 알아서 처리해주는 서비스를 비유할 수 있을 것 같습니다.
- 퀵배송 기사를 부르고 퀵 배송 접수증을 끊고 비용을 결제하는 등 관련 업무를 이 직원이 알아서 처리해주면 이 업무를 요청한 사람은 퀵 배송 과정을 신경쓸 필요가 없기 때문에, TCP와 UDP 통신을 소켓이 대리해서 해준다는 소켓의 역할과 유사합니다.

## 참고

- [RFC 793](https://datatracker.ietf.org/doc/html/rfc793)
- [Oracle Java Documentation](https://docs.oracle.com/javase/tutorial/networking/sockets/definition.html)
- Kurose, J. F., & Ross, K. W. (2017). Computer Networking: A Top-Down Approach. Pearson Education, Inc., p. 55