---
layout: post
title: Escaping VS Non-escaping
tags: 
- escaping
---
일단, 제목에서 "Non-escaping"이라고 썼지만, non-escaping이라는 어노테이션은 존재하지 않는다. escaping 어노테이션만 존재한다. @escaping이 붙지 않은 것을 non-escaping이라고 '지칭'하는 것일 뿐이다.  (@noescape라는 어노테이션이 있었지만 swift 3버전 부터 deprecated 되었다고 한다.)

Swift 공식 문서상 정의를 보면, 어떤 함수의 파라미터로 입력된 클로저가 함수 외부에서 실행이 될 때, 이 클로저를 escaping closure라고 한다.

이와 반대로, 어떤 함수의 파라미터로 입력된 클로저가 함수 내부에서 실행이 될 때, 이 클로저를 non-escaping closure라고 한다. (Swift 공식문서에는 나와 있지는 않지만 상반되는 개념이기 때문에...)

escaping을 더 명확하게 이해하기 위해 한가지 예를 생각해 보았는데, 아래와 같이 계획한 일만 수행하는 계획형인간 클래스를 만든다고 하자. 
```
class 계획형인간 {
    var 계획들: [() -> Void] = []
    
    func 계획추가(_ 계획: @escaping () -> Void) {
        계획들.append(계획)
    }
    
    func 계획수행() {
        for 계획 in 계획들 {
            계획()
        }
    }
}
```
'계획추가' 함수의 '계획' 파라미터 앞에 @escaping을 붙여주어야 한다. '계획' 클로저는 '계획추가' 함수에서 실행 되지 않고 함수 외부의 '계획수행' 함수에서 실행되기 때문이다.

만약 @escaping을 붙여주지 않으면 아래와 같이 컴파일 에러가 발생한다.
![nonescaping_compile_error](/assets/images/nonescaping_compile_error.png)

'그렇다면 왜 escaping 어노테이션이 필요할까? 그냥 클로저를 외부에서 실행하면 되는거 아닌가?' 라는 생각이 들었다. 그래서 곰곰이 생각해 보았는데, escaping 어노테이션이 필요한 이유는 함수 외부에서 호출할 수 없는 클로저를 컴파일 타임에 걸러내기 위함이라고 생각하였다. 클로저 내부에서 구조체의 프로퍼티를 접근하면 문제가 생길 수 있기에 이러한 경우를 막아주기 위해서라고 생각하였다.

예를 들어, 아래와 같이 과일창고라는 구조체를 만들고, 계획형 인간인 팀쿡에게 사과창고에 재고를 추가하라고 시킨다고 하자.
```
struct 과일창고 {
    var 재고: Int = 0
    
    mutating func 재고추가() {
        self.재고 += 1
    }
}

var 팀쿡 = 계획형인간()
var 사과창고 = 과일창고()
팀쿡.계획추가(사과창고.재고추가)
```
하지만 위 코드는 아래 이미지와 같이 컴파일 에러가 발생한다. 과일창고(사과창고)는 구조체이므로 value type 이기 때문에 @escaping 클로저의 파라미터로 입력될 수 없다. 즉, 과일창고는 구조체라서 과일창고의 재고 변수를 접근하는 것은 과일창고 매서드 외부에서는 불가능하다.
![struct_fruit_storage_compile_error](/assets/images/struct_fruit_storage_compile_error.png)

에러를 해결하려면 과일창고를 구조체에서 클래스로 변경해주어야 한다. 아래 이미지는 과일 창고를 클래스로 바꾼 후 정상 작동함을 보여준다.
![class_fruit_storage_compile_success](/assets/images/class_fruit_storage_compile_success.png)

결론은, 
- 함수의 클로저 파리미터가 함수 외부에서 실행 되어야 할 때 escaping 어노테이션을 사용한다.
- escaping 어노테이션을 사용함으로써 함수 외부에서 사용되지 못하는 클로저가 함수에 입력되는 것을 컴파일 타임에 제한할 수 있다.

참조: [Swift 공식 문서](https://docs.swift.org/swift-book/LanguageGuide/Closures.html)








