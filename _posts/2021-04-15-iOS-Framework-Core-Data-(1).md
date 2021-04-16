---
layout: post
title: "[iOS-Framework] 코어 데이터(Core Data) (1)"
date: 2021-04-15
category: read 
excerpt: ""


---

# [iOS-Framework] 코어 데이터(Core Data) (1)

> 1. Core Data: Core Data 개요(기능 및 설명)  
2. Creating a Core Data Model: Core Data Model 만들기

- Entity / Attribute 추가는 [다음 글](https://p0tt3r-ios.github.io/2021/04/15/iOS-Framework-Core-Data-(2))로...

## Core Data

---

### Overview  
- 한 개의 장치에서 데이터를 유지/캐시하거나, CloudKit을 통해 여러 장치에서  
    데이터를 동기화할 수 있도록 하는 프레임워크
  
- Core Data를 사용하면, 오프라인에서 어플리케이션의 영구적인 데이터를 저장하고  
    일시적인 데이터를 캐시할 수 있습니다.(숨길 수 있다?)
  
- 한 개의 iCloud 계정을 사용하는 다수의 기기에 데이터를 동기화하기 위해,  
    Core Data는 자동으로 CloudKit 컨테이너에 미러링합니다.
  
- Core Data의 Data Model editor을 통해, 데이터의 타입과 관계를 정의하고,  
    정의된 각각의 클래스를 생성합니다.

### Persistence  
- Core Data는 저장소에 데이터를 맵핑하는 세부 과정을 추상화합니다.  
    이는 Swift, Obj-C에서 데이터 베이스를 직접적으로 관리하지 않고,  
    쉽게 데이터를 저장하도록 해줍니다.

### Undo and Redo of Individual or Batched Changes  
- Core Data의 뒤로가기(undo) 관리자는 데이터 변화를 추적하고,  
    개별의 데이터를 전체적으로 또는 그룹별로 롤백할 수 있습니다.  
    
- 이 기능은 앱에서 redo, undo 기능을 쉽게 추가하도록 도와줍니다.

### Background Data Tasks  
- 객체에 JSON을 파싱(parsing)하는 등의 UI와 관련없는 데이터 작업을 백그라운드에서 수행합니다.  
  
- 수행 후 데이터를 저장 / 캐시하여 서버 왕복을 줄일 수 있습니다.  

### View Synchronization
- Core Data는 테이블 뷰 또는 컬렉션 뷰에 데이터 소스를 제공하여  
    유저의 뷰와 데이터를 동기화 상태로 유지하도록 도와줍니다.

### Versioning and Migation
- Core Data는 앱 발전(업데이트)에 따라 데이터 모델의 버전을 관리하고,  
    유저 데이터를 이전하는 매커니즘을 포함합니다.

## Creating a Core Data Model

---

### Overview  
- Core Data를 사용하기 위해서는 앱의 객체 구조를 담을 데이터 모델 파일을 생성해야 합니다.  
  
- 데이터 모델 파일을 생성하는데에는 두 가지 방법이 있는데,  
    새 프로젝트를 생성할 때, 그리고 이미 생성된 프로젝트에 추가입니다.

### Add Core Data to a New Xcode Project

- 먼저, 새 프로젝트를 생성할 때는 새 프로젝트 생성 창에서  
    하단 버튼 중 [Use Core Data]를 체크하여 프로젝트를 생성합니다.  
![https://user-images.githubusercontent.com/46529663/114820065-62817180-9df9-11eb-8dc1-0fe605f69e69.png](https://user-images.githubusercontent.com/46529663/114820065-62817180-9df9-11eb-8dc1-0fe605f69e69.png)

### Add Core Data Model to an Existing Project

- 이미 존재하는 프로젝트에 데이터 모델을 추가할 때는,  
    Xcode에서 File > New > File(Command + N)을 선택한 후,  
    iOS 템플릿에서 Data Model을 선택합니다.  
![https://user-images.githubusercontent.com/46529663/114820467-fce1b500-9df9-11eb-9952-65660806be74.png](https://user-images.githubusercontent.com/46529663/114820467-fce1b500-9df9-11eb-9952-65660806be74.png)

- 위 두 가지 방법을 사용해서 Data Model을 생성하면,  
    Project Navigator에서 파일을 확인할 수 있습니다.  
![https://user-images.githubusercontent.com/46529663/114820621-3c100600-9dfa-11eb-99a1-7338ebd3c086.png](https://user-images.githubusercontent.com/46529663/114820621-3c100600-9dfa-11eb-99a1-7338ebd3c086.png)  
(클래스 명은 Entity 명으로 설정되니, 파일 명은 Model, Data Model 등으로 설정하셔도 됩니다.)  

참고자료  
[Apple Developer Documentation: Core Data](https://developer.apple.com/documentation/coredata)  
  
[Apple Developer Documentation: Creating a Core Data Model](https://developer.apple.com/documentation/coredata/creating_a_core_data_model)
