# Reference Counting
- 참조 카운팅은 iOS 에서 객체지향 프로그래밍의 메모리 관리를 위해 사용되는 기술이다.
- 각각의 객체는 참조 카운트 라는 정수값을 갖고 있으며, 이 값을 해당 객체의 참조(Reference) 수를 나타낸다.
- 객체의 LifeCycle 을 관리하기 위한 중요한 메커니즘이다.


## 참조 카운트 작동 원리
1. 참조 생성 : 객체를 참조할 때 마다 해당 객체의 참조 카운트가 1 증가한다.
    ```swift
        let objectA = MyClass() // 참조 카운트: 1
        let objectB = objectA   // 참조 카운트: 2
    ```
2. 참조 해제 : 참조가 해제되면 참조 카운트가 1 감소한다.
    ```swift
        var objectB: MyClass? = MyClass()   // 참조 카운트: 1
        objectB = nil   // 참조 카운트 0, 객체가 메모리에서 해제됨
    ```
3. 객체 해제 : 참조 카운트가 0이 되면, 그 객체는 메모리에서 해제된다.


## 참조 카운팅의 문제점
1. 순환 참조
    - 두 객체가 강한참조 상태에 있다면 Reference Count 가 0이 되지 않아 메모리 누수가 발생할 수 있다.
    - 순환 참조 문제를 해결할 방법 [ 약한 참조 ] 를 알고있어야 한다.

    ### 예시 코드
    ```swift
        class Person {
            let name: String
            var friend: Person?  // 강한 참조
            
            init(name: String) {
                self.name = name
            }
            
            deinit {
                print("\(name) is being deinitialized")
            }
        }

        var alice: Person? = Person(name: "Alice")  // Alice 참조 카운트: 1, Bob 참조 카운트: 0
        var bob: Person? = Person(name: "Bob")  // Alice 참조 카운트: 1, Bob 참조 카운트: 1

        alice?.friend = bob  // Alice 참조 카운트: 2, Bob 참조 카운트: 1
        bob?.friend = alice  // Alice 참조 카운트: 2, Bob 참조 카운트: 2

        alice = nil  // Alice 참조 카운트: 1, Bob 참조 카운트: 2 (순환 참조로 인해 객체가 해제되지 않음)
        bob = nil  // Alice 참조 카운트: 1, Bob 참조 카운트: 1 (순환 참조로 인해 객체가 해제되지 않음)
    ```