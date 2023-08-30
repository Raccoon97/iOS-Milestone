
# iOS ARC( Automatic Reference Counting )

- 메모리 관리를 위한 매커니즘입니다.
- Objective-C 와 Swift 에 모두 적용됩니다.
- 자동으로 메모리를 관리하여 메모리 누수와 같은 문제를 방지합니다.

## ARC 의 기본 원칙
1. **참조 카운팅(Reference Counting)** : 각 객체에는 참조 카운트가 있습니다. 객체에 새로운 참조가 생기면 참조 카운트가 증가하고, 참조가 끊기면 감소합니다.
2. **객체 해제(Deallocation)** : 객체의 참조 카운트가 0이 되면, 해당 객체는 메모리에서 해제됩니다.

## ARC 의 키워드와 개념
- **강한 참조(Strong Reference)**: 기본적인 참조입니다. 이 참조를 사용하면 참조 카운트가 증가합니다.

    ```swift
    var a = MyClass()
    ```

- **약한 참조(Weak Reference)**: 참조 카운트를 증가시키지 않습니다. 주로 델리게이션(Delegation) 패턴이나 클로저(Closures)에서 순환 참조를 피하기 위해 사용됩니다.

    ```swift
    weak var delegate: MyDelegate?
    ```
- **Unowned Reference**: weak과 유사하지만, nil이 될 수 없습니다.
    ```swift
    unowned var parent: MyParentClass
    ```
- **Optional vs Non-Optional**: weak 참조는 항상 옵셔널(Optional)이어야 하며, unowned 참조는 옵셔널이 아니어야 합니다.
## ARC 와 순환 참조
- ARC는 "순환 참조(Circular Reference)" 문제를 완전히 해결해주지는 못합니다. 이는 `weak` 또는 `unowned` 참조를 통해 해결해야 합니다.

    ### 예시 코드
    - 아래 코드에서 `bestFriend` 프로퍼티는 `weak` 로 선언되어 있어 순환 참조를 방지하고 있습니다.
        ```swift

        class Person {
            var name: String
            weak var bestFriend: Person?

            init(name: String) {
                self.name = name
            }

            deinit {
                print("\(name) is being deinitialized")
            }
        }

        var alice: Person? = Person(name: "Alice")
        var bob: Person? = Person(name: "Bob")

        alice?.bestFriend = bob
        bob?.bestFriend = alice

        alice = nil // "Alice is being deinitialized"
        bob = nil // "Bob is being deinitialized"

        ```