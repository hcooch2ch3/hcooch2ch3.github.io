---
layout: post
title: <초안> Port vs Socket
tags:
  - Port
  - Socket
---

자주 사용하는 개념이지만 나의 말로 설명하지 못했던 내용이라서 공부할겸 한번 정리해보고자 한다.

일단 ChatGPT 한테 물어보자.

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

## RFC 793

```markdown
- port: The portion of a socket that specifies which logical input or output channel of a process is associated with the data.

- socket: An address which specifically includes a port identifier, that is, the concatenation of an Internet Address with a TCP port.
```

## Oracle Java Documentation

```markdown
A socket is one endpoint of a two-way communication link between two programs running on the network. A socket is bound to a port number so that the TCP layer can identify the application that data is destined to be sent to.
```

## 정리

## 참고

- [RFC 793](https://datatracker.ietf.org/doc/html/rfc793)
- [Oracle Java Documentation](https://docs.oracle.com/javase/tutorial/networking/sockets/definition.html)