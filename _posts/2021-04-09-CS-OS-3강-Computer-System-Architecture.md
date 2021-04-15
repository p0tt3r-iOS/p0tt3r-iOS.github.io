---
layout: post
title: "CS-OS 3강 Computer System Architecture"
date: 2021-04-09
category: read 
excerpt: ""

---

# [CS-OS] 3강 Computer System Architecture

- 인터럽트(Interrupt)란?

    CPU가 실행 중일 때, I/O Device 등 장치에 예외사항(동작 완료 등)이 발생하여 처리가 필요한 경우에  
    CPU에게 알려 처리할 수 있도록 하는 것

    반대개념: 폴링(Polling): I/O Device 등의 장치의 동작이 완료될 때까지 CPU가 반복해서  
    장치의 register 상태를 확인하는 방식

- 인터럽트 매커니즘을 사용하는 이유?  
    1. I/O 연산이 CPU 연산보다 현저히 느리기 때문에  
    2. CPU가 다른 일과 겹쳐 I/O 연산을 하면 데이터 손실이 발생할 수 있기 때문에  

- 인터럽트 종류  
    1. 하드웨어 인터럽트  
        - 일반적으로 CPU 외부 I/O Controller에서 발생시킨다.  
    2. 소프트웨어 인터럽트  
        - OS의 시스템 콜(system call)을 통해 실행된다.  

- 인터럽트 처리 과정  
    1. 현재 실행중인 작업(기계어 코드)을 마치고 CPU의 프로그램 카운터(PC) 값을 안전한 곳에 저장한다.  
    2. 현재 수행중인 프로세스를 중단하고 프로세스의 상태를 해당 프로세스의 PCB(Process Control Block)에 저장한다.  
    3. IRQ Number(Interrupt Request Number)을 통해 인터럽트 소스(인터럽트를 보낸 장치) 확인  
    4. Interrupt Vector Table에서 IRQ Number에 대응하는 함수의 주소를 받아오고 ISR로 점프하여 주소를 확인하고 수행  
    5. 끝나면 저장된 프로세스의 마지막 위치로 복귀

---

- Hardware Protection: 멀티 프로그래밍을 할 때, 자기에게 할당된 메모리가 아닌 다른 프로그램의 메모리에 침범하는 것을 방지하기 위해 아래와 같은 매커니즘을 사용한다.

- Dual Mode Operation  
    1. 방식: OS의 수행 방식을 두가지로 나눈다.  
    2. Process Status Register: 마이크로 프로세서(CPU) 내부의 레지스터로,  
    레지스터의 Mode bit가 현재 모드를 표현함  
    3. Kernel Mode: Privileged Instruction(특권 명령)을 수행시킬 수 있는 모드  
        - 특징: 모든 Memory 영역에 접근할 수 있다.  
    4. User Mode: Privileged Instruction(특권 명령)을 수행시킬 수 없는 모드  
    5. Mode Change 방식  
        1. Interrupt를 발생시키면 모드 비트가 무조건 1(user mode)에서 0(kernel mode)으로 바뀐다.  
        2. System call을 실행한다.(추후 프로세스 내용에 포함)  

- I/O Protection  
    - 방식: Port register에 접근하는 작업을 모두 Privileged Instruction으로 만든다.  
        (I/O와 관련된 모든 함수를 Kernel Mode에서 실행되게 한다.)  
    - I/O Protection을 하는 이유?  
        1. 여러 작업이 I/O Register에 값을 쓰게(write) 한다면 문제가 발생할 수 있다.  
            (Stateless하고, Correctness를 보장할 수 없다.)  
        2. 특정 작업이 I/O 자원을 독점한다면 문제가 발생할 수 있다.  
            (시스템을 효율적으로 사용할 수 없다.)
