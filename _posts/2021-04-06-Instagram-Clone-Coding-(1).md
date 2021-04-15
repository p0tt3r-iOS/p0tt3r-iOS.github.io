---
layout: post
title: "[iOS-Swift] 인스타그램 클론 코딩 (1)"
date: 2021-04-06
category: read 
excerpt: ""

---

# 인스타그램 클론 코딩 (1)

> 최근 Swift(UIKit) 코드에 익숙해지기 위해서 클론 코딩을 하고 있다.  
GitHub에서 소스 코드를 찾던 중 3가지 디자인 패턴(VIPER, MVC, MVVM)을 사용해서  
Instagram을 구현한 레파지토리가 있어서 클론을 구현하기 시작했고,  
오늘 구현하는 중 새로 배운 부분을 정리하려고 한다. :)  
Source Code: [dks333 / KDInstagram](https://github.com/dks333/KDInstagram)

  
## UI
---
#### 1. UIGestureRegonizer
- 공식문서: The base class for concrete gesture recognizers.  
제스쳐를 인식하는 기본 클래스로 초기화(Initializing)을 통해 제스쳐와 액션을 연결할 수 있다.

```swift
    // 초기화를 통해 animateHeart 메서드와 타겟을 연결한다.
    let doubleTap = UITapGestureRecognizer(target: self, action: #selector(animateHeart))
    
    // action을 실행시키기 위해 필요한 Tap수를 입력하고 제스쳐를 View에 추가해준다.
    // 이제 imageCollectionView를 두번 탭하면 animateHeart 메서드가 실행됨
    doubleTap.numberOfTapsRequired = 2
    imageCollectionView.addGestureRecognizer(doubleTap)
```

#### 2. Zero duration means no animation
- 애니메이션을 생성할 때, Duration을 0으로 만들면 애니메이션이 존재하지 않는 것과 같음  
(지속시간이 0이니까)

```swift
    // zero duration means no animation
    let duration: TimeInterval = (animated ? 0.3 : 0.0)
```

#### 3. Use other SubView in same SuperView at function
- 같은 SuperView 안에서 다른 View를 메서드 안에서 사용할 수 있다!  
기존에는 SuperView가 같아도 코드로 구현한 뷰는 다른 함수 내에서 불러올 방법이 없는 줄 알았는데,  
addSubView()를 통해 SuperView에 추가된 상태라 그런지 다른 메서드에서 사용 가능하다.  
위 내용은 따로 포스팅해서 링크를 아래에 추가할 예정!
  
## Swift
***
#### 1. 클래스 전용 프로토콜
- 프로토콜의 상속 리스트에 class를 추가해서  
해당 프로토콜이 클래스 타입에만 채택될 수 있도록 제한할 수 있다.

```swift
    protocol ProtocolName: class {}
```

#### 2. isNaN: Bool
- 공식문서: A Boolean value indicating whether the instance is NaN (“not a number”).  
인스턴스가 수가 아닐 때, 알려주는  Bool 타입 인스턴스로  
수를 표현하는 타입(Float, Double, Decimal 등)에 존재한다.  
(예상 외로 Int 타입에는 존재하지 않는다.)  
수를 표현하는 타입이 수가 아닐 수 있나? 라는 의문을 가지게 하는데,  
무한대(.infinite)는 수가 아니라서 isNaN 값을 true로 return 할 수 있다고 한다.  
그리고 nan 값은 == 연산자(equal-to operator)를 사용했을 때, 절대로 true를 반환하지 않는다.

```swift
    let x = 0.0
    let y = x * .infinity
    // y is a NaN

    // Comparing with the equal-to operator never returns 'true'
    print(x == Double.nan)
    // Prints "false"
    print(y == Double.nan)
    // Prints "false"

    // Test with the 'isNaN' property instead
    print(x.isNaN)
    // Prints "false"
    print(y.isNaN)
    // Prints "true"
```

#### 3. infinity
- 공식문서: Positive infinity.  
양의 무한을 의미하고, 특정 수를 표현하는 것이 아닌 무한의 상태를 의미함  
공식문서의 아래 Discussion에는 모든 유한의 수보다 크고 다른 무한의 값과 같다.  
(greater than all finite number and equal to other infinite values.) 라고 되어있음.

#### 4. abs(_:)
- 공식문서: Returns the absolute value of the given number.  
절대 값을 반환한다.

## Questions

---

#### 1. Hashable Protocol and hash(into hasher: inout Hasher) Method

- 공식문서: A type that can be hashed into a Hasher to produce an integer hash value.
정수 해시 값을 생성하기 위해 해셔(Hasher)로 해시될 수 있는 타입이라고 하는데,
해시에 대한 개념도 부족한 지금 우선 해시에 대해 알아보고 내용 정리가 필요하다.

    [Swift ) Hashable](https://zeddios.tistory.com/498)

    > 위 포스트를 참고해봤는데,
    우선 Hashable의 정의는 "정수 Hash 값을 제공하는 타입"이라고 한다.
    그래서 우리는 Hash를 왜 제공받아야 하나?
    이유는 우리가 주로 사용하는 해시 테이블 이라는 자료구조가 있는데,
    그리고 Set, Dictionary 등은 해시 테이블로 구현이 되기 때문에, 이 정수 Hash 값을 받아야 빠르게 원소를 찾을 수 있다.
    그래서 Set의 요소, 그리고 Dictionary의 Key는 Hashable한 값만 받을 수 있다.

#### 2. UIColor.systemTeal

- 공식문서: A teal color that automatically adapts to the current trait environment.
현재 특성 환경에 자동으로 적응하는 청록(Teal) 색상…

    systemBlue, systemGray처럼 그냥 청록색인건가?
    이해가 안되지만 다른 곳에서 코드로 구현해보면 쉽게 이해할 듯

    > systemTeal은 그냥 청록색이였다.(약간 하늘색 느낌)

#### 3. UIApplication.shared.windows.first?.addSubview()

- UIApplication의 단일 인스턴스(shared) 내에서 windows 배열의 첫번째 요소(현재 뷰)에 서브뷰를 추가한다.

    위와 같이 이해했는데, windows 문서에 보면 마지막 window는 배열의 가장 위에 존재한다고 되어있다.

    (The last window in the array is on top of all other app windows)

    그럼 현재 뷰라는 뜻은 아닌 것 같은데, 이것도 코드로 구현해보면 알 수 있으려나?...

    > iOS앱은 모든 View들의 컨테이너 역할을 하는 UIWindow 인스턴스를 하나 가지는데,  
    UIWindow는 UIView의 하위 클래스 임으로, Window 자체가 View이다.  
    그리고 windows.first는 제일 아래(Array에서 첫번째)에 있는 윈도우인 것 까지 이해를 했는데,  
    왜 window에 addsubview를 하는 지는 아직 모르겠다...  

#### 4. UIBezierPath: NSObject

- 공식문서: A path that consists of straight and curved line segments that you can render in your custom views.

    > 이건 정의를 봐도 모르겠는데… 내용을 한번 더 읽어봐야겠다.
    커스텀 뷰에서 줄 수 있는 직선과 곡선으로 이루어진 path
    베지어 곡선이라는 알고리즘을 이용해서 뷰를 그려주는 방법인데,
    이건 한번 하려면 날잡고 공부를 하고 정리해야할 듯 하다.
    우선 참고 링크는 아래와 같다.

    [](https://zeddios.tistory.com/814)

#### 5. addConstraints

- 아래 코드와 같이 Contraint를 주는 법은 공식 문서에서 본 적 있는데,
어떻게 구현하는 지 몰라서 못해봤는데, 따라서 몇번 구현해보면 나중에도 사용할 수 있을 것 같다!

    > 이 방식을 Visual Format Language라고 말하는 듯하다.
    추후에 사용할 때, 참고 문서는 아래와 같다.

    [Programmatically Creating Constraints](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)

```swift
    addConstraints(NSLayoutConstraint.constraints(
        withVisualFormat: "H:|[lineView]|",
        options: NSLayoutConstraint.FormatOptions(rawValue: 0),
        metrics: metrics,
        views: views)
    )
```

#### 6. UITableView.tableFooterView: UIView?

- 공식문서: The view that is displayed below the table’s content.

    테이블 컨텐츠 아래에 보일 뷰라는 의미로 아래와 같은 코드를 작성했는데,
    테이블 뷰 아래에 초기화된 UIView를 둔다는 말인가…?

    > tableView.tableFooterView = UIView()를 사용하지 않으면,
    테이블뷰의 빈셀도 유효한 셀 아래 모두 표시되지만,
    사용하면 유효한 셀 아래(TableView Footer)에 빈 뷰가 존재한다.

```swift
    tablewView.tableFooterView = UIView()
```

#### 7. Referencing Outlet

- 스토리보드에서 소스코드를 따라 구현하다 보니,
드래그를 통해 referencing outlet을 지정해놓은 경우가 있는데
어떤 경우에 이렇게 지정해서 사용하는 지 알아봐야겠다.

    > Object에 set되는 outlet이다.
    스토리보드 위의 각 View 요소들을 Object로 생각하고
    그 요소들을 가지는 View들은 해당 Object를 Outlet으로 가지고,
    Object들은 referencing Outlet으로 상위 뷰를 가지는 듯 하다.
    약간 이해가 덜 되어서 오개념이 있을 수 있다...
