---
layout: post
title: "[iOS-Swift] Firebase Auth Example 클론 코딩"
date: 2021-04-05
category: read 
excerpt: ""

---

# Firebase Auth Example 클론 코딩

> 현재 진행 중인 프로젝트에 Firebase를 통해 로그인을 구현하는데,
구글에서 예제 소스코드를 제공하여서 구글의 개발자는 어떻게 코드를 작성하는 지 알아보고,
나중에도 Firebase Auth은 사용할 것 같아서 클론 코딩을 해보았다.
아는 게 많이 없는 상태라, 클론 코딩을 하는 동안 많은 내용을 습득했고 그 배운 내용들을 정리해보려 한다.

## UI

---

### 1. UIColor.systemBackground: UIColor

- 공식문서: The color for the main background of your interface.
공식문서에 적힌 내용과 같이 인터페이스의 메인 백그라운드 컬러라고 하는데
쉽게 이야기 하면 Dark mode일 때, Black / Light mode일 때, White를 표시한다.
그래서 VC의 백그라운드 뷰의 색을 인터페이스 모드에 따라 Black/White로 변경하고 싶다면
ViewDidLoad 내에 아래 코드와 같이 작성하면 된다.

```
    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .systemBackground
    }
```

### 2. UITabBarController.selectedIndex: Int

- 공식문서: The index of the view controller associated with the currently selected tab item.
탭 바 내에서 선택된 탭 아이템과 연결된 뷰 컨트롤러의 인덱스라는 의미로
UITabBarController 내에는 viewcontrollers Array가 존재하고
selectedIndex를 통해 인덱스 값을 지정해주면 해당 뷰 컨트롤러로 이동

```
    func transitionToViewController(atIndex index: Int) {
        selectedIndex = index
    }
```

### 3. UIView.translatesAutoresizingMaskIntoConstraints: Bool

- 공식문서: A Boolean value that determines whether the view’s autoresizing mask is translated into Auto Layout constraints.
Autoresizing mask를 AutoLayout Contraints로 변환 후 적용 여부를 결정하는 인스턴스로 코드이며,
View를 구성할 때, 자주 볼 수 있는 코드다.
그 이유는 View를 Programmatically(코드로) 생성하게 되면 위 인스턴스의 최초 값은 true로 설정되고
인스턴스의 값이 true로 지정될 경우 Autoresizing mask가 View의 위치, 크기등을 모두 지정하기 때문에
별도로 Constraints를 지정하면 Autoresizing mask로 지정된 Contraint와 충돌을 일으킨다. 그래서, 뷰를 코드로 작성하면서 Constraint를 지정해주고 싶다면 위 인스턴스의 값을 false로 설정해야한다.

```
    lazy var newButton = UIButton()

    func setupNewButton() {
        addSubview(newButton)

        newButton.translatesAutoresizingMaskIntoConstraints = false

        newButton.centerXAnchor.constraint(equalTo: centerXAnchor).isActive = true
        newButton.topAnchor.constraint(equalTo: loginButton.bottomAnchor, constant: 5).isActive = true
    }
```

### 4. NSLayoutConstraint.activate(_ constraints: [NSLayoutConstraint])

- 공식문서: Activates each constraint in the specified array.
일반적으로 코드로 Contraint를 작성하면 NSLayoutContraint.isActive = true를 사용하는데,
위 메서드를 통해 여러 Contraints를 한번에 적용할 수 있음

```
    func setupNewButton() {
        addSubview(newButton)

        newButton.translatesAutoresizingMaskIntoConstraints = false

        NSLayoutConstraint.activate([
            newButton.centerXAnchor.constraint(equalTo: centerXAnchor),
            newButton.topAnchor.constraint(equalTo: loginButton.bottomAnchor, constant: 5)
        ])
    }
```

### 5. UIDevice.current.orientation.isLandscape: Bool

- 공식문서: Returns a Boolean value indicating whether the device is in a landscape orientation.
현재 디바이스가 가로 모드이면 true, 아니면 false를 반환
위 인스턴스를 통해 화면 모드에 따라 다른 Contraint 설정 가능
(세로 모드는 UIDevice.current.orientation.isPortrait)

```
    let constant: CGFloat = UIDevice.current.orientation.isLandscape ? 5 : 20

    newButton.topAnchor.contraint(equalTo: view.topAnchor, constant: constant).isActive = true
    }
```

### 6. viewWillTransition(to size: CGSize, with coordinator: UIViewControllerTransitionCoordinator)

- 공식문서: Notifies the container that the size of its view is about to change.
뷰 사이즈가 변할 때 호출되는 함수
Coordinator은 UIViewControllerTansitionCoordinator 프로토콜을 준수하며,
일반적으로 view가 present, dismiss될 때 UIKit이 자동으로 적용시킨다.

```
    override func viewWillTransition(to size: CGSize, with coordinator: UIViewControllerTransitionCoordinator) {
        super.viewWillTransition(to: size, with: coordinator)

        loginView.emailTopConstraint.constant = UIDevice.current.orientation.isLandscape ? 15 : 50
        loginView.passwordTopConstraint.constant = UIDevice.current.orientation.isLandscape ? 5 : 20
    }
```

### 7. textFieldShouldReturn(_ textField: UITextField) -> Bool

- 공식문서: Asks the delegate whether to process the pressing of the Return button for the text field.
return 처리를 할 지 Delegate에게 묻는 메서드지만,
TextField에서 Return을 누르면 호출되기 때문에 Return을 눌렀을 때, 발생시킬 Activity를 넣을 수 있다.
아래 예시는 Email과 Password를 넣는 텍스트 필드에서 Return을 눌렀을 때,
Password TextField로 갈 지, 키보드를 내릴 지 선택해서 실행하는 메서드이다.

```
    extension LoginController: UITextFieldDelegate {
        func textFieldShouldReturn(_ textField: UITextField) -> Bool {
            if loginView.emailTextField.isFirstResponder,
               loginView.passwordTextField.text!.isEmpty {
                loginView.passwordTextField.becomeFirstResponder()
            } else {
                textField.resignFirstResponder()
            }
            return true
        }
    }
```

### 8. touchesBegan(_ touches: Set, with event: UIEvent?)

- 공식문서: Tells this object that one or more new touches occurred in a view or window.
한 번 또는 여러 번의 터치가 발생했을 때, 불리는 메서드이며
내부에 터치 발생 후 실행할 내용을 넣을 수 있다.
일반적으로 아래 예시와 같이 TextField가 있는 뷰에서 endEditing 메서드를 통해 키보드를 내린다.

```
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        super.touchesBegan(touches, with: event)

        view.endEditing(true)
    }
```

### 9. 테이블 뷰 헤더 텍스트 크기/색상 변경 메서드

- viewForHeaderInSection 메서드를 이용해서,
UILabel 생성 및 Section별 Label의 Text(의 크기/색상)를 변경하는 메서드 실행

```
    func tableView(_ tableView: UITableView, viewForHeaderInSection section: Int) -> UIView? {
        var label = UILabel()
        let section = sections[section]
        config(&label, for: section)
        return label
    }

    private func config(_ label: inout UILabel, for section: DataSource.Section) {
        label.text = section.headerDescription
        label.textColor = .label
        label.font = UIFont.boldSystemFont(ofSize: 19.0)
    }
```

### 10. UINavigationBar에 이미지를 추가하는 메서드

- 프로필 이미지를 NavigationBar 우측에 원으로 띄우는 경우 사용할 수 있다.

```
    extension UINavigationBar {
        func addProfilePic(_ imageView: UIImageView) {
            let length = frame.height * 0.46
            imageView.clipsToBounds = true
            imageView.layer.cornerRadius = length / 2
            imageView.translatesAutoresizingMaskIntoConstraints = false

            addSubview(imageView)

            NSLayoutConstraint.activate([
                imageView.trailingAnchor.constraint(equalTo: trailingAnchor, constant: -15),
                imageView.bottomAnchor.constraint(equalTo: bottomAnchor, constant: -5),
                imageView.heightAnchor.constraint(equalToConstant: length),
                imageView.widthAnchor.constraint(equalToConstant: length)
            ])
        }
    }
```

### 11. UINavigationBar 이미지의 투명도를 스크롤 정도에 따라 설정

- TableView가 스크롤 될 때, 10번에서 추가한 이미지의 투명도를 변경할 수 있다.

```
    func tableViewDidScroll(_ tableView: UITableView) {
        adjustUserImageAlpha(tableView.contentOffset.y)
    }

    private var originalOffset: CGFloat?

    private func adjustUserImageAlpha(_ offset: CGFloat) {
        originalOffset = originalOffset ?? offset
        let verticalOffset = offset - originalOffset!
        userImage.alpha = 1 - (verticalOffset * 0.05)
    }
```

### 12. UITabelView.isScrollEnabled: Bool

- true일 때, 해당 테이블 뷰 스크롤 가능 / false일 때, 불가능
다음 예시와 같이 사용할 수 있다.(sections Array가 비어있을 경우, 스크롤 불가능)

```
    tableView.isScrollEnabled = !sections.isEmpty
```

### 13. UITableViewCell.accesoryVIew 설정

- 예시: cell이 수정 가능한 요소일 경우 연필 이미지를 셀 오른쪽 끝에 표시

```
    cell.accessoryView = item.isEditable ? editableImageView() : nil

    private func editableImageView() -> UIImageView {
        let image = UIImage(systemName: "pencil")?
            .withTintColor(.systemGrey, renderingMode: .alwaysOriginal)
        return UIImageView(image: image)
    }
```

## Swift

---

### 1. Init(Designated init)과 convenience init의 차이

- 편의 이니셜라이저(Convenience init)은 지정 이니셜라이저(designated init)가 선언되어 있어야 사용이 가능하다.
- Convenience init의 규칙: 같은 클래스 안의 다른 이니셜라이저를 반드시 호출해야한다.

Convenience init을 사용하는 이유: 초기화를 좀 더 쉽게 도와준다.
(지정 이니셜라이저는 클래스 내의 모든 프로퍼티를 초기화 해야하는 반면에, 
편의 이니셜라이저는 지정 이니셜라이저를 호출한 후 특정 프로퍼티를 재설정하거나, 
지정 이니셜라이저에 편의 이니셜라이저의 전달인자를 줄 수 있다.)

## Auth

---

### 1. Apple Login은 SHA256을 사용해서 통신한다.

## Questions

---

### 1. UILabel에 Didset을 지정해놓으면 loadView될 때 호출되는 지?

- 한 번 구현해보면 이해할 수 있을 것 같다.

이건 내가 예제를 잘못 이해했다.
클론은 스토리보드 없이 진행해서, 뷰에서 UI 요소들을 모두 초기화 해야 했고,
아래 코드를 보면 setSomeLabel() 메서드에서 someLabel = UILabel()이 실행될 때,
didSet에 들어있는 print가 실행된다.

```swift
        class ViewController: UIViewController {
            var someLabel: UILabel! {
                didSet {
                    print("someLabel did set")
                }
            }
            
            override func viewDidLoad() {
                super.viewDidLoad()
                setSomeLabel()
            }
            
            private func setSomeLabel() {
                someLabel = UILabel()
                someLabel.textColor = UIColor.blue
                someLabel.translatesAutoresizingMaskIntoConstraints = false
                view.addSubview(someLabel)
                
                NSLayoutConstraint.activate([
                    someLabel.centerXAnchor.constraint(equalTo: view.centerXAnchor),
                    someLabel.centerYAnchor.constraint(equalTo: view.centerYAnchor)
                ])
            }
        }
```

### 2. 다운 캐스팅은 어떤 방식으로 실행되는 지?

- 책에서 한번 봤는데, 아직 방식을 이해하지 못한 것 같다.

    때때로, 특정 클래스 타입의 변수 또는 상수가 해당 클래스의 인스턴스를 참조하지 않을 수도 있다.

    예를 들어 Snack을 부모 클래스, Doritos를 자식 클래스로 선언해놓았지만,
    Doritos가 Snack 클래스의 인스턴스 인 것 처럼 초기화 할 수도 있다.(아래 코드 참조)
    이렇게 선언을 했을 때, mySnack을 Doritos 타입으로 사용해야할 일이 생기면,
    mySnack을 Doritos 타입으로 변환해줘야 하는데, 이 때 사용하는 것이 다운캐스팅이다.

    ```swift
    class Snack {}

    class Doritos: Snack {
        var flavor: String

        init(flavor: String) {
            self.flavor = flavor
        }
    }

    let mySnack: Snack = Doritos(flavor: "cheeze")
    ```

    타입 캐스팅 연산자에는 두 종류가 있다.

    1. as?
        - 다운캐스팅이 실패하는 경우 nil을 반환한다.
        - 반환 타입이 Optional이다.
    2. as!
        - 다운캐스팅이 실패하는 경우 런타임 오류를 발생시킨다.
        - 반환 타입이 Optional이 아니다.

### 3. 탈출 클로저(@escaping)는 어떤 경우에 사용하는 지?

- 이것도 책에서 봤는데....

    탈출 클로저란? 함수의 전달인자로 전달한 클로저가 함수 종료 후에 호출될 때, 클로저가 함수를 탈출한다고 표현하고 이를 탈출 클로저라고 부른다.

    탈출 클로저 선언: 클로저를 매개변수로 갖는 함수를 선언할 때, 매개변수 이름(의 콜론:) 뒤에 @escaping 키워드를 명시한다.

    일반적으로 탈출 클로저는 비동기 작업을 실행하는 함수에 사용된다.
    비동기 작업을 실행하는 함수는 컴플리션 핸들러(completion Handler)를 전달인자로 받아오는데,
    기존 함수의 실행을 마치고 비동기적으로 함수를 실행할 때, 
    컴플리션 핸들러를 사용하기 때문에, 탈출 클로저로 선언해야 한다.

### 4. 매개변수 앞에 inout은 어떤 경우에 사용하는 지?

- 이것도 문법적인 내용이라, 책에서 본 기억이 있는데, 사용하면서 배우는 것 같다.

    일반적으로 함수의 전달인자로 값을 전달할 때는, 값을 복사해서 전달한다.
    (C언어의 포인터와 같이) 값이 아닌 참조를 전달하려면 입출력(inout) 매개변수를 사용한다.
    inout 매개변수를 전달할 때는 전달인자인 변수 또는 상수 앞에 &를 붙여서 표현한다.

### 5. hidesBottomBarWhenPushed: Bool 실행 방식

일반적으로는 departure View에서 prepare(for segue)를 통해 실행하지만,
예제에서는 destination View에서 아래와 같이 실행(코드를 확실하게 이해하지 못했음)
연산 프로퍼티(get, set) 내용 재확인

- 공식문서: A Boolean value indicating whether the toolbar at the bottom of the screen is hidden when the view controller is pushed on to a navigation controller.

```swift
    // Hides tab bar when view controller is presented
    override var hidesBottomBarWhenPushed: Bool { get { true } set {} }
```

공식문서의 내용처럼 ViewController가 push될 때, 위 프로퍼티를 부르면서 get 연산 프로퍼티로 인해 true 값을 받고 BottomBar를 숨기는 것 같다.
