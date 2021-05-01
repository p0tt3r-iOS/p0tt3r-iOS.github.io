---
layout: post
title: "Network Lecture 10: Network Layer (2)"
date: 2021-05-01
category: read 
excerpt: ""


---

# [CS-Network] Lecture 10: Network Layer (2)

## IP datagram format

---

이 중 중요한 요소를 살펴보면,  

- IP protocol version num: IP 프로토콜의 버전(현재 IPv4)  
- Length: 패킷의 전체 길이  
- Source IP Address  
- Destination IP Adderss  
- Checksum  
- Time to live(TTL)  
  
TCP와 IP의 헤더는 20Byte인데, APP 계층에서 보낸 Message에 40Byte의 오버헤드가   
발생한다고 생각하면 된다.  
  
![https://user-images.githubusercontent.com/46529663/116783419-9fbc5380-aac9-11eb-9a9e-38f619d61572.png](https://user-images.githubusercontent.com/46529663/116783419-9fbc5380-aac9-11eb-9a9e-38f619d61572.png)

## IP Address(IPv4)

---

- A unique 32-bit number(이론 상으로 2의 32제곱의 갯수의 IP를 만들 수 있다.)  
- Identifires an interface: 호스트에 있는(NFC) 네트워크 인터페이스를 지칭한다.  
- Represented in dotted-quad notation(점으로 4분할된 표기법으로 표시한다.)  
  
    ![https://user-images.githubusercontent.com/46529663/116783412-992ddc00-aac9-11eb-86bc-dae15343f5e8.png](https://user-images.githubusercontent.com/46529663/116783412-992ddc00-aac9-11eb-86bc-dae15343f5e8.png)

### Grouping Related Hosts

- 인터넷은 Inter - network이다.  
    - 이 의미는, 호스트끼리 연결된 것이 아닌 네트워크를 서로 연결하였다는 뜻으로  
    각각의 네트워크를 사용하기 위해서 주소가 필요하다.

### Scalability Challenge

- IP를 규칙없이 배정하면 가장 쉽지만, 라우터 안에 들어있는 포워딩 테이블이  과도하게 커진다.  
- 그렇기 때문에, 계층화를 시켜놓았다.

### Hierarchical Addressing: IP Prefixes

- 네트워크 부분과 호스트 부분으로 계층을 나눈다.  
- 앞 부분 24-bit(12.35.158.0/24)는 네트워크를 표시하고, 뒷부분 8-bit는 호스트를 표시한다.

### IP Address and 24-bit Subnet Mask

- Subnet ID란, IP에서 어디까지가 Prefix(Network portion)인지 나타낸 것이다.  
IP 주소와 Subnet Mask(서브넷 마스크)는 언제나 같이 다닌다.  
  
    ![https://user-images.githubusercontent.com/46529663/116784300-7fdb5e80-aace-11eb-9724-5f870257fc99.png](https://user-images.githubusercontent.com/46529663/116784300-7fdb5e80-aace-11eb-9724-5f870257fc99.png)

### Scalability Improved

- 같은 네트워크에 속한 호스트들은 같은 Prefix를 가진다.  
- 이렇게 되면 포워딩 테이블이 단순해진다.  
  
    ![https://user-images.githubusercontent.com/46529663/116784389-ef514e00-aace-11eb-9370-2a9b85d4b9ca.png](https://user-images.githubusercontent.com/46529663/116784389-ef514e00-aace-11eb-9370-2a9b85d4b9ca.png)
  
- 이제 호스트 아이디는 라우터에 등록되지 않기 때문에, 어떤 값이 들어가도 상관없다.

## Classful Addressing

---

- 과거에는 IP 주소를 Class로 나누었다.  
예를 들어, Class A는 /8, B는 /16, C는 /24와 같이 Prefix를 배정했다.  
- 하지만 위와 같이 주소를 배정하면 Class A는 255개의 기관만 사용할 수 있고,  
2의 24제곱 개의 호스트를 가질 수 있는데 이는 비효율적이라,  
90년대 중반 Classless로 전환되었다.

## Classless Inter-Domain Routing(CIDR)

---

- 2개의 32-bit number을 통해 네트워크를 나타낸다.  
Network number = IP address + mask  
- 내가 필요한 개수 만큼 네트워크 Prefix의 크기를 유연하게 사용할 수 있다.
  
    ![https://user-images.githubusercontent.com/46529663/116784680-8cf94d00-aad0-11eb-8ce3-7477ade442d9.png](https://user-images.githubusercontent.com/46529663/116784680-8cf94d00-aad0-11eb-8ce3-7477ade442d9.png)

## Longest Prefix Match Forwarding

---

- 라우터의 포워딩 테이블에서 Prefix가 가장 많이 매칭될 때, 포워딩한다.  
- 이런 일을 반복적으로 하는 것이 라우터가 하는 일

## Subnets

---

- 같은 Prefix를 가진 디바이스의 집합(= 라우터를 거치지 않고 접근할 수 있는 호스트의 집합)  
- 라우터는 인터페이스가 있기 때문에 IP 주소를 가진다.(인터페이스 별로 IP주소를 가짐)  
그리고 각각의 IP에 Prefix가 다르며, 이는 여러 개의 Subnet에 걸쳐있다는 뜻이다.  
라우터와 라우터 사이의 공간도 Subnet으로 본다.(같은 Prefix를 가진다.)  
  
- 초기에 IPv4를 만들 때는, 2의 32제곱 개의 IP가 충분하다고 생각했지만  
90년대 중반 인터넷이 상업화 되면서 주소공간이 모자랄 것을 대비하여 IPv6를 만든다.  
(IPv6는 128-bit로 2의 128제곱(지구 상의 모래알 개수보다 많다.))  
- 하지만 현재는 IPv4를 사용하고 있다.(이미 주소공간은 초과했지만 서로 공유하고 있다.)  

## Network Address Translation(NAT)

---

주소 공간 고갈 후 현재 사용하고 있는 방식(트릭)이다.  

- 네트워크 내부에서는 유일한 IP 주소를 사용하되, 다른 네트워크에서도 해당 IP를 사용할 수 있다.  
- 유일한 IP는 네트워크 외부로 나가면 안되기 때문에, NAT 기능을 하는 게이트웨이 라우터에서 유일한 IP 주소로 변환해준다.(포트넘버 포함)  
  
    ![https://user-images.githubusercontent.com/46529663/116785441-853ba780-aad4-11eb-9a1f-d2be8842e555.png](https://user-images.githubusercontent.com/46529663/116785441-853ba780-aad4-11eb-9a1f-d2be8842e555.png)

- IP 주소를 타고 돌아올 때는, 포트 넘버를 통해 호스트를 찾는다.  
- 문제점  
    - NAT에서 IP 주소를 바꾸는 것과 동시에 포트 넘버도 바꾸는데,  
    포트 넘버는 TCP, 즉 Transpot 계층의 Data지만 네트워크 계층(라우터)에서 변경하면서  
    Layering violation(계층 침범)이 발생한다.  
    - 원래 포트 넘버의 기능은 호스트 내부에서 프로세스를 찾아갈 때 쓰는 용도인데,  
    이 포트 넘버를 호스트를 찾을 때 사용하게 되면서 프로세스를 찾지 못한다.  
