---
layout: post
title: "[iOS-Framework] 코어 데이터(Core Data) (3)"
date: 2021-04-16
category: read 
excerpt: ""


---

# [iOS-Framework] 코어 데이터(Core Data) (3)

Core Data Model을 만든 후, 본격적인 구현은 Core Data Stack을 만들면서 부터 시작됩니다.  
> Setting Up a Core Data Stack: 코어 데이터 스택 세팅하기

## Setting Up a Core Data Stack

---

### Core Data Stack
저도 Core Data Stack 부분 이해하는데, 시간이 오래걸렸는데, 이걸 이해 하셔야 Core Data를 알고 사용할 수 있습니다.  

- Core Data는 앱의 모델 계층을 관리, 유지하기 위해 몇가지 클래스를 제공합니다.  
    이러한 클래스를 통틀어 Core Data Stack이라 하는데, 아래와 같습니다.  
    
    1. **NSManagedObjectModel**: Core Data Stack에서 접근할 데이터를 나타냅니다.  
    
    2. **NSManagedObjectContext**  
        - 위 객체는 앱이 가장 많이 상호작용하는 객체이며, 앱 전체에 노출됩니다.  
        - 영구 저장소에서 객체를 가져올 때, 임시 복사본으로 가져와 수정할 수 있고 저장하지 않는 한 영구 저장소의 데이터는 변경되지 않는다.  
        - 모든 managed objects는 managed object context에 등록되어야 합니다.  
            컨텍스트(Context)를 사용하여 개체 그래프에 개체를 추가하고, 삭제할 수 있습니다.  
        - 컨텍스트는 각 개체의 Attrubutes와 개체 간의 관계의 변경 사항을 추적합니다.  
            추적을 통해 컨텍스트는 undo(뒤로가기)와 redo(다시하기) 기능을 제공할 수 있습니다.  
            
    3. **NSPersistentStoreCoordinator**  
        - 영구 저장소(NSPersistentStore)에서 앱 타입의 인스턴스를 저장하고, 가져옵니다.  
            (영구 저장소는 디스크에 있을 수도, 메모리에 있을 수도 있습니다)  
        - NSManagedObjectModel 개체가 초기화 된 후 생성됩니다.  
        - 위 객체는 영구 저장소에서 데이터를 인식하면  
            해당 데이터를 요청하는 NSManagedObjectContext로 전달합니다.  
        - NSPersistentStore을 추가하는 호출은 비동기적으로 수행됩니다.  
            (동기적 수행 시, 예외상황에서 UI 스레드를 멈춰버리는 등의 상황이 발생할 수 있음)  
            
    4. **NSPersistentContainer**  
        - Core Data Stack 생성을 처리합니다.  
            (Model, Context, Coordinator을 한 번에 설정합니다.)  
        - NSManagedObjectContext에 대한 접근과 여러 편리한 메서드를 제공합니다.  

참고자료  
그냥 Documentation만 찾으면... 내용이 부족하고, Documentation Archive를 찾아야합니다.  
[Apple Developer Documentation: Initializing the Core Data Stack](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/InitializingtheCoreDataStack.html#//apple_ref/doc/uid/TP40001075-CH4-SW1)

### Initailize a Persistent Container

- 일반적으로 Core Data는 앱이 실행될 때, 초기화 시킵니다.  
    우선, persistent container를 처음 호출할 때 까지 초기화를 지연시키도록 lazy 변수로 선언해줍니다.  

- 프로젝트를 생성할 때, Core Data를 선택했다면, 아래 코드는 자동으로 생성되지만,  
    선택하지 않았다면 직접 작성해야합니다.  
    1. AppDelegate 파일에 NSPersistentContainer 타입의 lazy 변수를 선언합니다.  
    2. 변수에 persistent container 인스턴스를 생성하고, 데이터 모델 파일 이름과 이니셜라이저를 넘겨줍니다.  
    3. loadPersistentStores 메서드를 통해, 영구(Persistent) 저장소를 불러옵니다.  
        만약 저장소가 없다면, 생성합니다.  

    ```swift
    import CoreData

    class AppDelegate: UIResponder, UIApplicationDelegate {
        ...
        lazy var persistentContainer: NSPersistentContainer = {        
            let container = NSPersistentContainer(name: "DataModel")
            container.loadPersistentStores { description, error in
                if let error = error {
                    fatalError("Unable to load persistent stores: \(error)")
                }
            }
            return container
        }()
        ...
    }
    ```

    - persistent container가 한번 생성되면 model, context, store coordinator  
        각 인스턴스에 대한 참조를 managedObjectModel, viewContext, persistentStoreCoordinator 프로퍼티에 담는다.  

### Pass a Persistent Container Reference to a View Controller

- UI에서 container을 참조하기 위해, container의 참조를 view로 넘겨줍니다.  
    우선, Core Data를 import하고, persistent container을 참조하기 위해   
    앱의 root view controller(가장 아래 뷰(메인 뷰인 경우가 많음))에 아래와 같이 변수를 선언합니다.  

    ```swift
    import UIKit
    import CoreData

    class ViewController: UIViewController {
        // 아래 코드와 같이 persistent container를 참조하는 변수 선언
        var container: NSPersistentContainer!

        override func viewDidLoad() {
            super.viewDidLoad()
            guard container != nil else {
                fatalError("This view needs a persistent container.")
            }
            // 이 아래 작성하는 코드는 persistent container가 사용 가능한 경우 실행됩니다.
        }
    }
    ```

- AppDelegate 파일로 돌아와서 application(_:didFinishLaungchingWithOptions:) 메서드 내부에  
    해당 앱의 root view controller 타입으로 다운 캐스팅한 window?.rootViewController를  
    rootVC 변수에 넣습니다.  
  
    약간 말이 어렵지만, window?.rootViewController를 ViewController(제일 아래 뷰) 타입으로   
    다운 캐스팅(ViewController의 프로퍼티인 container을 사용하기 위해)해서 변수에 넣어준다고 이해하면 좋습니다.  

    ```swift
    class AppDelegate: UIResponder, UIApplicationDelegate {
        ...
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
            if let rootVC = window?.rooViewController
        }
        ...
    }
    ```

### Subclass the Persistent Container

- NSPersistentContainer은 자식 클래스(subclass)로 사용되는 걸 선호합니다.  
    자식 클래스는 Core Data와 연관된 코드를 편리하게 넣을 수 있는 장소입니다.

    ```swift
    import CoreData

    // PersistentContainer이 NSPersistentContainer의 자식 클래스(subclass)
    // 이렇게 선언하면 편하게 saveContext 함수를 사용할 수 있다.
    class PersistentContainer: NSPersistentContainer {    
        func saveContext(backgroundContext: NSManagedObjectContext? = nil) {
            let context = backgroundContext ?? viewContext
            guard context.hasChanges else { return }
            do {
                try context.save()
            } catch let error as NSError {
                print("Error: \(error), \(error.userInfo)")
            }
        }    
    }
    ```

---

- 추후에 Core Data를 백그라운드에서 받아올 때, 참조할 문서
    [Apple Developer Documentation: Using Core Data in the Background](https://developer.apple.com/documentation/coredata/using_core_data_in_the_background)
