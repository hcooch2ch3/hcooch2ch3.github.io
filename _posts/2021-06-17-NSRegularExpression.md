---
layout: post
title: NSRegularExpression
tags: 
- NSRegularExpression
- regex
---

정규표현식을 필요할때 마다 주먹구구식으로 공부해서 사용하곤 했었는데, 이번 기회에 정확히 이해해서 사용해보고자 이 글을 쓰게 되었다. 

정규표현식은 'Regular Expression' 이라는 영단어를 그대로 번역한 말인 것 같다. 그래서 단어만 봤을때 정확히 무슨 기술인지 감이 오지 않는다. 여기저기 찾아본 결과 정규표현식은 문자열에서 특정 패턴을 찾으려 할 때, 패턴을 표현하기 위한 규칙이라고 정의할 수 있을 것 같다. 아래는 정규 표현식에 대한 위키 백과의 정의이다.

> 정규 표현식 또는 정규식은 특정한 규칙을 가진 문자열의 집합을 표현하는 데 사용하는 형식 언어이다. 정규 표현식은 많은 텍스트 편집기와 프로그래밍 언어에서 문자열의 검색과 치환을 위해 지원하고 있으며... 중략

익숙한 정규표현식의 사용예시는 회원가입 기능 구현에서의 사용일 것 같다. 회원가입 기능을 구현할 때 아이디 또는 비밀번호의 텍스트 필드에 올바른 값이 입력되었는지 확인할 때 사용 할 수 있을 것이다. 아래는 Zoom의 회원가입 화면인데, 여러개의 비밀번호의 규칙을 사용자에게 안내한 후 사용자가 잘못된 입력을 하면 즉각적으로 피드백을 보낸다.

![zoom signup](/assets/images/zoom_signup.png)

회원가입 뿐만 아니라, 문자열에서 특정 패턴을 찾아야한다면 어디서든 다양하게 사용할 수 있을 것이다. (당장 다른 예시가 떠오르는건 없지만..) 

Swift에서의 정규표현식 사용은 NSRegularExpression Class을 사용함으로써 가능하다. NSRegularExpression의 객체를 생성한 후, 생성된 객체의 match 관련 메서드를 호출함으로써 문자열에 특정 패턴이 존재하는지 여부를 찾아낼 수 있다. match 관련 메서드는 numberOfMatches(in:options:range:),  matches(in:options:range:) 등이 있다.

## NSRegularExpression init

NSRegularExpression의 init 메서드에 대한 Definition은 다음과 같다.

```
init(pattern: String, 
options: NSRegularExpression.Options = []) throws
```

pattern과 options, 이 2개의 파라미터를 넣어줘야 NSRegularExpression 객체를 만들 수 있다. 첫번째 파라미터인 pattern은 정규표현식을 문자열로 만들어서 넣어주면 된다. 그리고 두번째 파라미터인 options에는 탐색할때 적용하고 싶은 options을 넣으면 된다. 두 파리미터 모두 객체가 만들어진 이후에는 변경이 불가능하다. 또한 pattern의 정규표현식에 문제가 있으면 객체 생성 도중 에러를 던진다. 아래는 잘못된 정규표현식으로 인해 에러가 발생한 것이다.

![regex error](/assets/images/regex_error.png)

에러를 발생시키지 않으려면, 그리고 원하는 문자열을 잘 탐색하기 위해서 정규표현식의 문법을 잘 이해해야겠다.

## 정규표현식의 구성요소

정규표현식은 Character, Metacharacter 그리고 Operator로 구성된다. Character는 단어 그대로 a, b, c, 1, 2, 3 같은 문자 그 자체로서 패턴을 구성한다. Metacharacter는 Character 이외의 패턴을 구성하기 위한 문자이며, 문자 그 자체로서 패턴을 구성하지 않고 치환되어 패턴을 구성하는 문자이다. Operator는 패턴을 구성하기보다 패턴내에서 특정 기능을 수행하기 위한 문자이며, Character와 Metacharacter 빈도수를 나타내는 등의 기능을 한다. 위키백과에서는 Operator와 Metacharacter을 구분짓지 않고 모두 Operator로 분류하지만, 애플 공식문서에는 Operator와 Metacharacter가 따로 분류되어있다. 

정규표현식의 구성요소에 대해 간단하게 정리하자면,
- Character: a, b, c, 1, 2, 3 같은 문자.
- Metacharacter: 문자 이외의 문자. 실제로는 치환되어 패턴을 구성한다. 예를 들어 '\n'은 개행(new line)으로 치환된다.
- Operator: 패턴에서 문자의 빈도수 등 특정 기능을 수행하는 연산자. 

그리고 자주 사용될 것 같은 Metacharacter와 Operator을 다음의 표로 정리해보았다. 

|기호|설명|예시|
| :----: | :----: |-|
|   .   |   모든 문자 중 1개   |문자열: "abcd", 패턴: "." 일 때, numberOfMatches는 4이다. "."은 모든 문자와 일치하기 때문에 "a", "b", "c", "d" 4개의 문자와 일치한다.|
|( )|문자 그룹|문자열: "abcd", 패턴: "(abc)" 일 때, numberOfMatches는 1이다. "abc"는 "abcd"문자열에서 1개만 일치하기 때문이다.|
|\||문자 그룹 내에서의 or|문자열: "abcd", 패턴: "(ab\|cd)" 일 때, numberOfMatches는 2이다. 패턴은 "ab" 또는 "cd"와 일치하는가를 의미하고, "ab" 1번과 "cd" 1번해서 총 2번 일치하기 때문이다. |
|[ ]|[ ] 내부 문자 중 1개|문자열: "abcd", 패턴: "[abc]" 일 때, numberOfMatches는 3이다.  패턴은 "a" 또는 "b" 또는 "c"와 일치하는 문자의 개수를 의미하며, 이 중 "a", "b", "c" 세개가 일치하기 때문이다.|
|[^]|[ ] 내부 문자 이외의 문자 중 1개|문자열: "abcd", 패턴: "[^abc]" 일 때, numberOfMatches는 1이다. 패턴은 "a" 그리고 "b" 그리고 "c"와 일치하지 않는 문자의 개수를 의미하며, 이 중 "d" 한 개가 일치하기 때문이다.|
|{n}|n개의 문자|문자열: "aaabaacaaad", 패턴: "a{3}" 일 때, numberOfMatches는 2이다. 패턴 "a{3}" 은 "a" 연속 3개를 의미하며, 문자열에는 "aaa"가 두번 포함되어 있기 때문이다.|
| \\d | 숫자 중 1개 |문자열: "0a1b2c3d4", 패턴: "\\d" 일 때, numberOfMatches는 5이다. 패턴은 숫자가 몇개 포함되어있는가를 의미하며, 문자열에는 "0", "1", "2", "3", "4", 총 5개의 숫자가 포함되어 있기 때문이다.|
| \\D| 숫자를 제외한 문자 1개 |문자열: "0a1b2c3d4", 패턴: "\\D" 일 때, numberOfMatches는 4이다. 패턴은 숫자 이외의 문자가 몇개 포함되어있는가를 의미하며, 문자열에는 "a", "b", "c", "d", 총 4개의 문자가 포함되어 있기 때문이다.|
| \\w | word 중 1개, [A-Za-z0-9_] 와 동일 |문자열: "0)a1!b2@c3#d4$", 패턴: "\\w" 일 때, numberOfMatches는 9이다. 14개의 문자 중 특수문자 5개, ")", "!", "@", "#", "$", 을 제외하면 9개가 남기 때문이다.|
| \\W | word가 아닌것중 1개, [^A-Za-z0-9_] 와 동일 |문자열: "0)a1!b2@c3#d4$", 패턴: "\\W" 일 때, numberOfMatches는 5이다. 문자, 숫자, 공백을 제외하면 특수문자 5개, ")", "!", "@", "#", "$", 만 남기 때문이다.|

이외에도 다양한 Metacharacter와 Operator가 있다.

## Match 관련 메서드

문자열에서 정규표현식을 탐색하기 위해서 다음과 같은 5개의 match 관련 메서드를 사용할 수 있다.

| 함수            | 설명 |
| --------------- | ---- |
| numberOfMatches(in:options:range:) | 문자열에 패턴이 몇개 포함되어있는지 반환해주는 함수 |
|enumerateMatches(in:options:range:using:)|문자열에서 패턴과 일치하는 모든 부분(배열)을 순회하면서 입력받은 block을 실행하는 함수|
|matches(in:options:range:)|문자열에서 패턴과 일치하는 모든 부분(배열)을 반환하는 함수|
|firstMatch(in:options:range:)|문자열에서 패턴과 일치하는 첫번쩨 부분을 반환하는 함수|
|rangeOfFirstMatch(in:options:range:)|문자열에서 패턴과 일치하는 첫번째 부분의 범위를 반환하는 함수|

검색 후 바로 치환하도록 하는 replaceMatches(in:options:range:withTemplate:) 등의 메서드도 있지만, 이는 다음에 다시 공부해보려고 한다.

## Options

NSRegularExpression에는 두 종류의 옵션이 존재한다. NSRegularExpression의 객체를 생성할때 사용되는 옵션과, matching 관련 함수를 호출할때 사용하는 옵션 두가지이다. 객체 생성할때 필요한 옵션은 NSRegularExpression.Options 타입이고, 패턴(정규표현식)에 대한  세부적인 옵션을 설정하는 것이다. matching 관련 함수를 호출할때 필요한 옵션은 NSRegularExpression.MatchingOptions 타입이며, matching할 때 세부적인 옵션을 설정하기 위해 사용된다. 각각의 세부설명은 아래와 같다.

### NSRegularExpression.Options

| 옵션            | 설명 |
| --------------- | ---- |
|caseInsensitive| 패턴의 대, 소문자를 구분하지 않고 탐색   |
|allowCommentsAndWhitespace| 패턴 내에 공백과 #으로 시작하는 comment를 허용 |
|ignoreMetacharacters| 패턴의 메타문자도 일반 문자로 취급하여 탐색 |
|dotMatchesLineSeparators| 패턴의 '.'이 모든 문자 뿐만 아니라 개행문자도 매치되도록 탐색|
|anchorsMatchLines| 패턴의 '^'와 '$'이 각각 라인의 시작과 끝으로 매칭되도록 탐색(원래는 문자열의 시작과 끝만 일치하지만)|
|useUnixLineSeparators|'\n'만을 개행으로 취급하여 탐색(원래는 이외의 모든 표준 개행문자를 개행으로 취급)|
|useUnicodeWordBoundaries|유니코드 TR#29을 word boundary로 취급하여 탐색(원래는 전통적인? 정규표현식의 word boundary을 사용)|

### NSRegularExpression.MatchingOptions

| 옵션           | 설명 |
| -------------- | ---- |
|reportProgress| enumerateMatches에서만 작동하는 옵션이며, 문자열을 순회하면서 패턴 매칭여부를 확인할때마다(매칭이 되고 안되고 상관없이) block을 호출하는 옵션 |
|reportCompletion|enumerateMatches에서만 작동하는 옵션이며, 문자열을 순회하면서 패턴이 매칭 됐을때만 block을 호출하는 옵션|
|anchored|문자열의 시작부분이 패턴과 일치해야만 검색을 진행하며, 검색 중 일치하지 않는 부분이 나오면 검색을 중지하는 옵션. (문자열의 시작부터 연속적으로 일치하는 것만 찾는 옵션)|
|withTransparentBounds|문자열에 word boundary(\\b)가 range 이외 영역에 명시적으로 있는지 확인하는 옵션 (이외의 옵션은 패턴에 \\b가 있을 경우, range 바깥은 word boundary로 취급한다. range가 문자열 전체이면 효과가 없음.)|
|withoutAnchoringBounds|문자열의 시작과 끝(^, $)이 range 이외 영역에 없다고 설정하는 옵션 (이외의 옵션은 패턴에 ^, $가 있을 경우, range 바깥은 ^, $ 로 취급한다. range가 문자열 전체이면 효과가 없음.)|

## 응용 - Zoom 비밀번호 유효성 확인 기능 구현
글 첫부분에서 언급했던 Zoom의 회원가입 페이지에서, 비밀번호 텍스트 필드에 올바른 비밀번호가 입력되었는지 확인하는 기능을 정규표현식을 이용하여 구현해보고자 한다.

Zoom 회원가입의 비밀번호 조건은 아래와 같다.
- 8자 이상이여야 함.
- 문자(a, b, c...) 1개 이상 포함.
- 숫자(1, 2, 3...) 1개 이상 포함.
- 대문자 및 소문자 모두 포함.

구현을 위해 위의 조건을 좀 더 가다듬으면 다음과 같을 것이다.
- 비밀번호 총 길이는 8자 이상.
- 영문 소문자 1개 이상 포함.
- 영문 대문자 1개 이상 포함.
- 숫자 1개 이상 포함.
- 특수문자는 제한 없음.

그리고 위 조건을 코드로 구현해보았다. 일단 guard문을 통해 8개 이상인지 확인을 하고, 정규표현식을 통해 소문자 1개 이상, 대문자 1개 이상, 숫자 1개 이상이 포함되면 true를 반환하고 아니면 false를 리턴하도록 아래와 같이 isValid 함수를 구현하였다.

```swift
func isValid(password: String) -> Bool {
    guard password.count >= 8 else {
        return false
    }
    let pattern = "[a-z]+[A-Z]+[0-9]+"
    let regex = try! NSRegularExpression(pattern: pattern, options: [])
    let range = NSRange(location: 0, length: password.count)
    return regex.numberOfMatches(in: password, options: [], range: range) == 1 ? true : false
}

let password = "aA00000000"
isValid(password: password) ? print("Right Password") : print("Wrong Password")
```
하지만 이 구현은 테스트 케이스를 몇개 넣어본 결과, 제대로 작동하지 않음을 확인할 수 있었다.
소문자, 대문자, 숫자 순으로 구성된 비밀번호만 true를 리턴하고 이외의 순서 조합은 false로 반환하는 문제가 있었다. (예를 들어 "01ABccd+"은 숫자, 대문자, 소문자... 순으로 구성된 올바른 패스워드인데 false를 반환한다.)

그래서 조사를 더 해보면서 Look-ahead라는 Operator를 이용하면 된다는 것을 발견하였다. (이 [사이트](https://www.ocpsoft.org/tutorials/regular-expressions/password-regular-expression/)에 정답이 있었다... 허무하다.) 그리고 아래와 같이 다시 수정해 보았다.

```swift
func isValid(password: String) -> Bool {
    let pattern = "^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z]).{8,}$"
    let regex = try! NSRegularExpression(pattern: pattern, options: [])
    let range = NSRange(location: 0, length: password.count)
    return regex.numberOfMatches(in: password, options: [], range: range) == 1 ? true : false
}

let password =  "01ABccd+"
isValid(password: password) ? print("Right Password") : print("Wrong Password")
```
이 코드는 여러 테스트케이스에서도 잘 작동하는 것을 확인할 수 있었다. 또한 이 코드는 정규표현식 자체에서 글자수를 확인하기 때문에 별도의 글자수를 확인하는 조건문이 필요하지 않다는 장점이 있다.

이 코드의 정규 표현식을 자세히 한번 풀어서 설명해보고자 한다.
```swift
let pattern = "^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z]).{8,}$"
```
- 정규표현식의 "^"와 "$"은 문자열의 시작과 끝을 의미한다.
- "(?=...)"은 Look-ahead(전방 탐색)라고 한다. 전방탐색이란 "(?=...)"와 일치하는 부분을 찾으면 그것을 기준으로 앞부분이 일치하는지를 탐색하는 것이다. 
- Look-ahead에 대한 자세한 설명은 [민소네](http://minsone.github.io/regex/regexp-lookaround)님 블로그를 보고 자세하게 알 수 있었다. 하지만 쉽지 않은 개념이라서 이에 대한 상세한 설명은 아직 쓰지 못할 것 같다.
- 어쨌든 (?=.*[0-9])은 문자열 전체에서 숫자가 1개 있는지 확인한다. 마찬 가지로 이하의 "(?=...)"들은 소문자와 대문자가 1개 있는지 확인한다.
- 그리고 세개의 Look-ahead(전방 탐색)를 모두 만족하면 맨 마지막의 ".{8,}"를 통해 문자가 8개 이상인지 확인한다. (각각 1개만 찾으면 1개 이상이 있는 것을 만족하는 것이므로) 
- 요약하면 문자열의 시작과 끝까지 숫자 1개, 소문자 1개, 대문자 1개가 있는지 확인하고 총 8개의 문자가 있는 것을 확인하게 된다. 


## 마무리
 
간단하게 요약해보자면, Swift에서 정규표현식을 사용하기 위해서는 정규표현식 패턴과 옵션을 입력한 NSRegularExpression 객체를 생성하여야 한다. 생성된 객체의 여러 match 메서드를 이용하여 정규표현식 패턴과 문자열이 매치되는지 확인할 수 있다. 또한 NSRegularExpression은 다양한 정규표현식 옵션, 다양한 match 메서드, 그리고 다양한 match 옵션을 제공한다.

정규표현식을 제대로 사용하기 위해서는 Metacharacter와 Operator의 사용방법에 대해서 자세히 알아야 한다. 특히 Look-around Operator(Look-ahead, Look-behind)을 이용하여 정규표현식을 더 잘 사용할 수 있다.

## 참고
- 정규표현식 정의: <https://ko.wikipedia.org/wiki/%EC%A0%95%EA%B7%9C_%ED%91%9C%ED%98%84%EC%8B%9D>
- Zoom 회원가입 화면: <https://zoom.us/>
- NSRegularExpression 애플 공식문서: <https://developer.apple.com/documentation/foundation/nsregularexpression>
- Regular Expression: <https://en.wikipedia.org/wiki/Regular_expression>
- What is .WithTransparentBounds?: <https://stackoverflow.com/questions/37605358/what-is-withtransparentbounds>
- Password Regular Expression: <https://www.ocpsoft.org/tutorials/regular-expressions/password-regular-expression/>
- 전방탐색과 후방탐색: <http://minsone.github.io/regex/regexp-lookaround>
