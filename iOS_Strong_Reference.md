# Strong Reference

- 객체지향 프로그래밍에서 다른 객체를 참조하는 가장 기본적인 방법이다. 
- 강한 참조를 사용하여 객체를 참조하면, 해당 객체의 참조 카운트(Reference Count)가 1 증가한다. 
- 참조 카운트가 0이 되기 전까지는 객체가 메모리에서 해제되지 않는다.

## 강한 참조의 특징
1. 참조 카운트 증가 : 강한 참조를 만들면 참조되는 객체의 참조 카운트가 자동으로 증가한다. 이로 인해 객체는 메모리에서 살아남게 된다.

2. 소유관계 : 강한 참조는 일종의 "소유관계"를 만든다. 즉, 참조하는 객체가 참조되는 객체를 "소유"하고 있어서, 참조하는 객체가 사라지거나 참조를 해제하기 전까지는 참조된 객체도 함께 유지된다.

## 강한 참조의 문제점
1. 순환 참조 : 강한 참조만을 사용하여 두 객체가 서로를 참조하게 되면, 두 객체의 참조 카운트가 감소되지 않아 메모리 누수가 발생할 수 있다. 이러한 현상을 순환 참조(Circular Reference)라고 한다.

2. 메모리 누수 : 강한 참조 때문에 참조 카운트가 제대로 감소하지 않으면, 필요 없는 객체까지 메모리에 계속 남아 있을 수 있다. 이러한 상황을 메모리 누수(Memory Leak)라고 한다.

### 예시 코드
```swift
Copy code
class Dog {
    var name: String
    init(name: String) {
        self.name = name
    }
}

var myDog: Dog? = Dog(name: "Buddy")
var anotherDog = myDog  // 강한 참조

myDog = nil
// anotherDog가 여전히 myDog가 참조하던 객체를 강하게 참조하고 있으므로 객체는 메모리에서 해제되지 않음.
```