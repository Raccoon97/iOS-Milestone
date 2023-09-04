# Weak Reference

- 약한 참조(Weak Reference)는 참조 카운팅(Reference Counting)에 영향을 미치지 않는 참조 방법이다. 
- 다른 객체를 참조하되, 참조 카운트를 증가시키지 않아 메모리 누수나 순환 참조를 예방하는 데 유용하다.


## 약한 참조의 필요성
1. 순환 참조 방지: 두 객체가 서로를 참조하는 경우 참조 카운트가 0이 되지 않아 메모리에서 해제되지 않는 문제(순환 참조)를 해결한다.

2. 메모리 효율성: 약한 참조를 사용하면 필요 없어진 객체는 즉시 메모리에서 해제되어 메모리 사용량을 최적화할 수 있다.


## 약한 참조와 강한 참조의 차이

1. 참조 카운트의 영향: 강한 참조는 참조 카운트를 증가시키지만, 약한 참조는 그렇지 않습니다.

2. 옵셔널 타입: 약한 참조는 항상 옵셔널 타입으로 선언됩니다. 이는 참조하고 있는 객체가 메모리에서 해제될 수 있기 때문입니다.


### 예시코드

```swift
class MyClass {
    var name: String
    init(name: String) {
        self.name = name
    }
}

class AnotherClass {
    weak var myClassInstance: MyClass?
}

var myClass: MyClass? = MyClass(name: "Example")
var anotherClass = AnotherClass()
anotherClass.myClassInstance = myClass

// 약한 참조는 참조 대상이 메모리에서 해제될 수 있으므로, 이를 염두에 두고 옵셔널 바인딩 등을 사용하여 안전하게 접근해야 합니다.
if let myClass = anotherClass.myClassInstance {
    print("Name is \(myClass.name)")
} else {
    print("myClassInstance has been deallocated")
}

myClass = nil  // MyClass 인스턴스가 메모리에서 해제됨

```

## 약한 참조의 한계
- 약한 참조는 참조하는 객체의 생명주기에 영향을 미치지 않기 때문에, 참조하는 객체가 더 이상 존재하지 않을 수 있다. 따라서 약한 참조를 사용할 때는 항상 이를 체크해야 한다.

- 약한 참조는 iOS에서 순환 참조 문제를 해결하거나 메모리 관리를 더 효율적으로 할 수 있게 도와주는 중요한 기술이다. 약한 참조를 잘 이해하고 사용하면, 메모리 누수와 같은 문제를 효과적으로 방지할 수 있다.



## 추가 사항

### unowned 참조
- Swift에서는 약한 참조를 weak 뿐만 아니라 unowned 키워드를 사용해서도 선언할 수 있다. 
- unowned는 weak과 유사하지만 몇 가지 차이점이 있다.
- Non-Optional: unowned 참조는 Non-Optional 형태이다. 즉, nil이 될 수 없으므로 사용하기 전에는 반드시 값이 존재한다고 가한다.
- unowned는 항상 값을 가져야 하므로, 선언과 동시에 초기값이 필요하다.

### UI 요소와의 관계
- UIKit 또는 SwiftUI 등의 UI 프레임워크에서 약한 참조는 자주 사용된다. 
- 뷰 컨트롤러가 뷰를 참조할 때 또는 델리게이션 패턴을 사용할 때 약한 참조가 효율적인 메모리 관리를 도와준다.

### 디버깅과 트러블슈팅
- 약한 참조를 사용할 때, 객체가 예상치 못하게 nil이 되어 발생하는 문제를 디버깅하는 것이 중요할 수 있다. 
- 이 경우, 디버거나 프로파일러를 사용하여 메모리 누수나 참조 카운트 문제를 찾을 수 있다.
