---
layout: post
title: "Network Lecture 4: Application Layer (2)"
date: 2021-04-22
category: read 
excerpt: ""


---

# [CS-Network] Lecture 4: Application Layer (2)

## Reliable Data Transfer

App 계층에서 내려온 데이터가 Transport 계층을 통해 다른 App에 에러 / 데이터 유실없이 전달되는 것

---

실제적으로 Transport 계층 아래 계층은 에러 또는 데이터 유실이 발생할 수 있다.

- Unreliable한 환경에서 어떤 결과가 일어나는가?  
    - Message error(패킷 에러)  
    - Message loss(패킷 유실)
  
    위 두가지만 해결하면 reliable한 환경을 제공할 수 있다.

## Build simple reliable Data Transfer Protocol

단순한 Realiable Data Transfer은 아래와 같이 구현할 수 있는데,

---

- RDT 1.0  
    - 에러, 유실이 발생하지 않는 아래 계층을 통해 데이터를 주고 받는다.(비현실적인 상황)  
    - 필요한 매커니즘이 없다.

- RDT 2.0  
    - 패킷 에러가 발생 가능한 아래 계층을 통해 데이터를 주고 받는다.   
    - 필요한 매커니즘   
        - Error detection  
            - checksum bits를 추가해서 구현한다.  
        - Feedback  
            - Acknowedgements(ACKs): 리시버가 센더에게 데이터가 올바르게 왔다고 알린다.  
            - Negative acknowledgements(NAKs): 리시버가 센더에게 패킷에 에러가 있다고 알린다.  
        - Retransmission  
            - 센더가 NAK를 받으면 패킷을 재전송한다.    
            - Feedback이 올 때, 에러가 발생한 경우에도 재전송하게 되는데   
                 이러한 경우 리시버는 2개를 보냈는지, 에러에 대한 재전송인지 알 수 없기 때문에   
                중복에 대한 컨트롤이 필요하다.  
                - Sequence number(이 매커니즘이 포함되면 RDT 2.1이 된다.)  
                    - 중복에 대한 컨트롤을 위해 센더는 보내는 패킷마다 Sequence number을 추가한다.  
- RDT 2.1  
    - Sender  
        - Add Sequence number  
        - Check if ACK/NAK corrupted(변형 여부)  
        - NAK 또는 corrupted feedback에 대한 재전송  
    - Receiver  
        - 받은 패킷이 중복되었는 지 확인  
        - 받은 패킷이 변형된 경우, NAK 발송(성공적으로 받으면 ACK 발송)

- RDT 2.2  
    - RDT 2.1과 같지만, 2.2는 NAK 없이 ACK만 사용한다.  
    - ACK에 Sequence number을 추가해서 마지막에 받은 정상적인 패킷의 Sequence number을 보내준다.  
        Ack가 보내준 Sequence number가 보낸 넘버와 다를 때, 재전송한다.

- RDT 3.0  
    - 패킷 에러와 유실 모두 발생 가능한 계층을 통해 데이터를 주고 받는다.  
    - 패킷 유실(Packet loss)를 확인하기 위한 매커니즘: 타이머

- Principles of Reliable Data Transfer  
    - 신뢰할 수 없는(Unreliable) 계층에서는 어떤 일이 일어나는가?  
        - Packet error, Packet loss  
    - Packet Error를 위한 매커니즘   
        - Error detection, feedback, retransmission, sequence #  
    - Packet Loss를 위한 매커니즘  
        - Timeout(timer)  
    - 위 내용은 간단한 Reliable Transfer Protocol이지만, 실제로 사용하는 프로토콜은 훨씬 복잡하다.  
        하지만 동일한 원칙으로 구현된다.  
        - 예를 들어, 여기서 가정한 상황은 데이터를 한 비트씩 보내는 것이지만,  
            실제로 TCP는 한꺼 번에 다수의 데이터 패킷을 보내고, 피드백을 받는다.
