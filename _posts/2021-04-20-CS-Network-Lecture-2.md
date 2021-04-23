---
layout: post
title: "Network Lecture 2: Introduction (2)"
date: 2021-04-20
category: read 
excerpt: ""


---

# [CS-Network] Lecture 2: Introduction (2)

- 네트워크 계층: App - Transport - Network - Link - Physical  
- Network apps: 네트워크 기능이 있는 프로세스  
    - e-mail, web 등 네트워크를 사용하는 어플리케이션  
    - 실제적으로 네트워크를 사용할 때, App 계층만 사용하는 경우가 대부분이다.

## Client-server architecture

---

### Server

- 언제나(24시간) 호스트를 한다.  
- 고정된 IP 주소를 가진다.

### Clients

- 서버와 통신한다.  
- 간헐적으로 연결된다.  
- 대부분 동적 IP 주소를 가진다.

## Process communicating

---

- Process 간의 통신(inter-process)을 위해 OS가 인터페이스(시스템 콜)를 제공한다.  
- 다른 컴퓨터에 존재하는 Process와의 통신도 이와 다르지 않다.(OS에서  인터페이스(Socket)를 제공한다.)

## Sockets

---

- 소켓 간의 통신을 위해서는 연결하고자 하는 소켓의 주소를 알아야한다.  
- 소켓의 주소의 인덱싱: IP Address, Port number

## What transport service does an app need?

---

App 계층에서 필요로 하는 Tranport 계층의 기능

### Data integrity

- 데이터가 유실되지 않고 온전성을 유지하는 기능

### Timing

- 시간 제약을 줄 수 있는 기능(내가 보낸 데이터가 몇 ms 내에 도착해야 한다.)

### Throughput

- 처리 속도(시스템 효율)을 요구하는 기능(내가 보내준 데이터가 일정량 이상의 처리 속도를 가져야 한다.)

### Security

- 암호화, 데이터 온전성 등을 요구하는 기능

위 네가지 필요사항 중 Transport 계층이 제공하는 기능은 Data integrity 밖에 없다.(TCP)  
제공해주지 않는 기능들은 App 계층에서 구현하여 사용한다.

## HTTP overview

---

### HTTP: Hypertext transfer protocol

### Client/sever model

- Client: Request하고, Response를 receive한다. Web object들을 화면에 출력한다.  
- Sever: 웹 서버는 Object들을 Request에 대한 Response로 보낸다.  
- HTTP는 Request / Response로 존재한다.

### Use TCP

- HTTP로 Request / Response 하기 이전에 TCP Connection을 먼저 생성해야 한다.

### HTTP is "stateless"

- 단순히 요청이 들어오면 디스크에서 읽어서 보낸 후, 다른 요청을 처리한다.  
    그렇기 때문에 Client에 대한 정보를 가지고, 유지하지 않는다.

## HTTP connections

---

HTTP가 TCP를 사용하는 방식에 따라 2가지로 나뉘어 진다.  
(Request - reponse 후에 TCP 연결을 끊으면 non-persistent, 끊지 않고 재사용하면 persistent)

## Terminology

---

- IP Address(IP 주소): 인터넷 상에 존재하는 컴퓨터의 주소  
- Port(포트): IP 주소를 지칭하더라도, 해당 컴퓨터에 프로세스가 많은데 특정 프로세스를  나타내는 것이 포트 / DNS는 IP 주소만 반환하기 때문에 포트 넘버는 암묵적 동의를 통해 모두 똑같이(:80) 사용
