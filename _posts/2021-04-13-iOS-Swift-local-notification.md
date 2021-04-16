---
layout: post
title: "[iOS-Swift] 로컬 푸시 알림(local notification)"
date: 2021-04-13
category: read 
excerpt: ""


---

# [iOS-Swift] 로컬 푸시 알림(local notification)

개인 프로젝트(Plank On)을 진행하면서, 추가하려는 기능 중 하나가  
유저가 원한다면 매일 특정 시간에  플랭크를 해야한다는 알림을 띄우려고 한다.  
    
처음에 찾았을 때는 iOS는 APNs를 사용해서 알림을 보내기 때문에 서버가 필요하다고 봤는데,  
찾아보니 위의 내용은 Server notification이고, 내가 원하는 기능은 local notification으로 구현이 가능하다!  
    
이 포스트에서는 Local notification을 몇 가지 카테고리로 나누어서 내용을 정리해보려 한다.  
1. 간단한 구현 방법
2. 스케쥴링 방식

아래는 우선 첫 번째로 알람을 설정하는데 꼭 필요한 메서드만 구현한 내용이다.  
1. 앱의 알림(notification)과 관련된 작업을 관리하는 중심 객체인   
    UNUserNotificationCenter 클래스의 싱글톤 인스턴스 사용

    ```swift
    let center = UNUserNotificationCenter.current()
    ```

2. User's device에 알림을 보내기 위한 권한 요청 메세드 추가

    ```swift
    func requestAuthNotification() {
        let notificationAuthOptions = UNAuthorizationOptions(arrayLiteral: [.alert, .badge, .sound])
        center.requestAuthorization(options: notificationAuthOptions) { success, error in
            if let error = error {
                print("Error: \(error.localizedDescription)")
            }
        }
    }
    ```

3. 알림 보내기 요청 메서드 추가

    ```swift
    func requestSendNotification(seconds: Double) {
        let content = UNMutableNotificationContent()
        content.title = "알림 제목"
        content.body = "알림 내용"
            
        let trigger = UNTimeIntervalNotificationTrigger(timeInterval: seconds, repeats: false)
        let request = UNNotificationRequest(identifier: UUID().uuidString,
                                            content: content,
                                            trigger: trigger)
            
        center.add(request) { error in
            if let error = error {
            print("Error: \(error.localizedDescription)")
            }
        }
    }
    ```

4. 권한 요청 및 알림 발송

    ```swift
    // 예시이기 때문에, 권한 요청 및 알림 설정을 viewDidLoad에 추가함
    override func viewDidLoad() {
        super.viewDidLoad()

        // 뷰 로드가 되면 User에게 알림 권한 요청
        // 권한이 있을 때는 다시 띄우지 않음
        requestAuthNotification()
        // 3초 후에 알림이 뜨도록 설정(요청)
        requestSendNotification(seconds: 3)
    }
    ```

5. AppDelegate에 UNUserNotificationCenterDelegate 메서드 추가

    ```swift
    // MARK: - UNUserNotificationCenterDelegate Method

    extension AppDelegate: UNUserNotificationCenterDelegate {
        func userNotificationCenter(_ center: UNUserNotificationCenter,
                                    willPresent notification: UNNotification,
                                    withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
            completionHandler([.alert, .badge, .sound])
        }
        
        func userNotificationCenter(_ center: UNUserNotificationCenter,
                                    didReceive response: UNNotificationResponse,
                                    withCompletionHandler completionHandler: @escaping () -> Void) {
            completionHandler()
        }
    }
    ```

6. AppDelegate를 UNUserNotification의 Delegate로 설정

    ```swift
    @main
    class AppDelegate: UIResponder, UIApplicationDelegate {
        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
            UNUserNotificationCenter.current().delegate = self
            
            return true
        }
    }
    ```

두 번째로, 애플 공식문서에 있는 알림 스케쥴링 방식이다.

1. 알림 전송을 위해 상태를 특정해야한다.  
    여기서 상태를 특정하라는 말이 조금 말이 이상한데,   
    날짜(+ 시간)으로 설정할 지, 현재부터 특정 시간 후로 설정할 지, 지역으로 설정할 지 선택하라는 말이다.  
    - UNCalendarNotificationTrigger - 특정한 날(그리고 시간)에 알림을 발송하는 트리거
    - UNTimeIntervalNotificationTrigger - 특정 시간 후에 알림을 발송하는 트리거
    - UNLocationNotificationTrigger - 특정 장소에 들어가거나 나올 때 알림을 발송하는 트리거

    내가 필요한 기능은 매일 같은 시간에 알림을 발송하는 거라,  
    아래 예시에선 UNCalendarNotificationTrigger을 사용한다.  
    (트리거마다 다른 매개변수를 가지기 때문에, 다른 트리거를 사용할 때는 아래 예시와 다를 수 있다.)

2. 매주 순환하는 트리거 생성

    ```swift
    // Set Notification Time
    var dateComponents = DateComponents()
    dateComponents.calendar = Calendar.current

    dateComponents.hour = hour
    dateComponents.minute = minute
    // 특정 요일에 알림을 보내고 싶다면 아래 코드를 작성해야한다.
    // dateComponents.weekday = 3 
    // 1부터 일요일, 3은 화요일

    // weekday를 추가하지 않고 repeat를 true로 설정하면 매일 알림을 보낸다.
    let trigger = UNCalendarNotificationTrigger(dateMatching: dateComponents, repeats: true)
    ```

3. 위 트리거를 request에 매개변수로 주고 UNNotificationCenter.current().add(_:)로 추가한다.

    ```swift
    // Create the request
    let uuidString = UUID().uuidString
    let request = UNNotificationRequest(identifier: uuidString,
                                        content: content,
                                        trigger: trigger)
            
    // Schedule the request with the system.
    // 위에서 center를 let center = UNNotificationCenter.current()와 같이 정의함
    center.add(request) { error in
        if let error = error {
            print(error.localizedDescription)
        }
    }
    ```

참고자료  
[Apple doc: Scheduling a Notification Locally from Your App](https://developer.apple.com/documentation/usernotifications/scheduling_a_notification_locally_from_your_app)  
[Apple doc: Scheduling and Handling Local Notifications](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/SchedulingandHandlingLocalNotifications.html#//apple_ref/doc/uid/TP40008194-CH5-SW1)

이 외의 방법은 첫번째 구현 방식과 같고,   
위와 같이 트리거를 적용하면 매주 설정한 시간에 알림이 발생한다.  
아래 코드는 ViewController에서 정의한 매일 특정 시간에 알림을 보내는 메서드이다.

```swift
func requestSendNotification(time: Date) {
    // Configure Notification Content
    let content = UNMutableNotificationContent()
    content.title = "알림 제목"
    content.body = "알림 내용"
        
    // Set Notification Time
    var dateComponents = DateComponents()
    dateComponents.calendar = Calendar.current
        
    let dateFormatter = DateFormatter()
    dateFormatter.dateFormat = "HHmm"
    
    // 아래 Index 사용 부분은 extension으로 subscript를 구현해야 가능하다.
    let hourString = dateFormatter.string(from: time).substring(toIndex: 2)
    let minuteString = dateFormatter.string(from: time).substring(fromIndex: 2)
    
    guard let hour = Int(hourString), let minute = Int(minuteString) else { return }
        
    dateComponents.hour = hour
    dateComponents.minute = minute
        
    let trigger = UNCalendarNotificationTrigger(dateMatching: dateComponents, repeats: true)
        
    // Create the request
    let uuidString = UUID().uuidString
    let request = UNNotificationRequest(identifier: uuidString,
                                        content: content,
                                        trigger: trigger)
        
    // Schedule the request with the system.
    center.add(request) { error in
        if let error = error {
            print(error.localizedDescription)
        }
    }
}
```

공식문서를 보면 알림(notification)에 특정 상호작용이나 커스텀 액션을 구현할 수 있는데,  
이 내용은 나중에 참고해봐야겠다.  
[Apple doc: Managing Your App's Notification Support](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/SupportingNotificationsinYourApp.html#//apple_ref/doc/uid/TP40008194-CH4-SW1)
