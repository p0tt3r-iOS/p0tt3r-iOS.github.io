---
layout: post
title: "[iOS-Framework] 코어 데이터(Core Data) (2)"
date: 2021-04-15
category: read 
excerpt: ""


---

# [iOS-Framework] 코어 데이터(Core Data) (2)

> 1. Configuring Entities: Entity 추가하기  
2. Configuring Attributes: Attributes 추가하기

## Configuring Entities

---

### Entity란?

- Entity는 이름, 속성(Attirbute), 관계 등을 포함한 객체  
    (쉽게 이해하면 데이터로 활용되는 class)

### Add Entities  
Core Data model을 추가한 후, .xcdatamodeld 파일에서 아래와 같이 Entity를 추가한다.  
- 좌측 하단에 있는 Add Entity 버튼을 누른다.  
- 좌측 상단 ENTITIES List에 추가된 새 Entity의 이름을 바꿔준다.

### Configure Entities  
Entity를 추가한 후 우측 Inspector bar의 data inspector을 보면 설정할 수 있는 부분있다.  
내용이 많지만 내게 필요한 Codegen만 정리하고 넘어가려 한다.  

- Codegen  
    Code generation의 약자로 선택된 옵션에 따라 Entity를 지원하기 위해   
    managed object subclass와 프로퍼티 파일을 생성한다.  
    3가지 값을 선택할 수 있는데,  
    1. Class Definition(기본 값)  
        - **생성된 로직이나 프로퍼티를 수정할 필요가 없는 경우 사용하는 옵션**  
        - 이 옵션을 사용할 경우, 생성된 소스코드 파일이 프로젝트에 보이지 않는다.  
        - Xcode는 클래스와 프로퍼티 파일을 빌드 할 때 생성하고, 빌드 디렉토리에 넣어놓는다.  

    2. Category/Extension  
        - **Managed object subclass에 Logic을 추가하고 싶을 때 사용하는 옵션**  
        - 이 옵션을 선택하면 Subclass 파일을 생성해야 하는데, 방법은 아래와 같다.  
            1. 상단 메뉴 바에서 Editor > Create NSManagedObject Subclass 선택  
            2. 해당 Entity를 선택하고 Next > Next > Create 버튼 클릭  
            3. Create 버튼을 클릭하면, Class와 properties 파일이 생성되는데,  
                Properties 파일은 삭제한다.  
                (위 옵션에서도 프로퍼티는 빌드 할 때 생성되기 때문에)  
                ![https://user-images.githubusercontent.com/46529663/114827680-f9ebc200-9e03-11eb-99a6-73e006ca5752.png](https://user-images.githubusercontent.com/46529663/114827680-f9ebc200-9e03-11eb-99a6-73e006ca5752.png)
                생성하고 나면, 프로젝트 네비게이터에 아래와 같이 나타난다.  
                ![https://user-images.githubusercontent.com/46529663/114827765-0ec85580-9e04-11eb-99d9-a9551e96d9e3.png](https://user-images.githubusercontent.com/46529663/114827765-0ec85580-9e04-11eb-99d9-a9551e96d9e3.png)

    3. Manual/None  
        - **Managed object subclass의 로직이나 프로퍼티를 수정해야 할 때, 사용하는 옵션**  
        - 이름과 같이 이 옵션을 사용하면 Core Data는 파일을 생성하지 않고, 개발자가 수동(Manually)으로 파일을 생성해야한다.  
        - 파일 생성 방법은 Category/Extension과 같고, 프로퍼티 파일을 삭제하지 않고 사용한다.  

참고자료  
[Apple doc: Configuring Entities](https://developer.apple.com/documentation/coredata/modeling_data/configuring_entities/)  

[Apple doc: Generating Code](https://developer.apple.com/documentation/coredata/modeling_data/generating_code)

## Configuring Attributes  

---

### Attribute란?  
- Entity에 들어가는 프로퍼티(class 내의 프로퍼티의 개념이랑 비슷하다.)  

### Add Attributes  
- Entiry를 선택하고 우측 하단의 Add Attributes 버튼을 누른다.  
- 새로 추가한 Attribute의 이름을 설정하고, Type(기본값: Undefined)을 설정한다.  
- 추가할 옵션이 있다면 우측 Data Model Inspector에서 확인하고 수정한다.  
    ![https://user-images.githubusercontent.com/46529663/114831112-f823fd80-9e07-11eb-946a-5619bd53b946.png](https://user-images.githubusercontent.com/46529663/114831112-f823fd80-9e07-11eb-946a-5619bd53b946.png)
