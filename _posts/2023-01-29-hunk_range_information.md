---
layout: post
title: Hunk Range Inforamtion
tags:
  - git
---

소스코드를 수정 후 git diff를 하면 아래와 같은 결과가 나온다.

![git_diff](/assets/images/git_diff.png)

하루에도 수십번은 git diff를 하는데, 문득 아래 문장이 정확히 어떤 의미인지 궁금해졌다.

```
@@ -1,3 +1,5 @@
```

그래서 이것의 의미를 찾아보고 정리해보고자 한다.

일단 이것의 이름은 Hunk Range Information 이라고 한다.

그리고 이런식으로 Hunk를 표현하는 방식을 Unified format 이라고 한다.

Context format이라는 것도 있는데, Context format은 Hunk Range Information을 아래와 같이 표현한다고 한다.

```
*** 1,3 ****
--- 1,5 ----
```

그래... 그럼 Hunk는 뭘까?

> When comparing two files, diff finds sequences of lines common to both files, interspersed with groups of differing lines called hunks.

Hunk란 두개의 파일을 비교할때, 두 파일의 다른 부분을 나열한 그룹이라고 한다. 즉, 소스코드의 변경사항이 있는 부분과 그 주변 부분의 코드들이라고 이해하면 될 것 같다.

그래서 Hunk Range Information이란, Hunk가 포함하고 있는 내용의 범위 정보를 보여주는 것이라고 할 수 있을 것이다.

소스코드가 방대해지고 수정하는 곳이 많아지면 그만큼 Hunk도 많이 생겨날 것이다. 이때 Hunk가 몇번째 라인부터 시작하는지, 몇 줄의 소스코드를 포함하는지를 보여주기 위한 정보가 Hunk Range Information 이다.

그렇다면 이제 이것을 세부적으로 어떻게 이해해야하는지 정리해보자.

```
@@ -1,3 +1,13 @@
-import UIKit
+import Foundation

 var greeting = "Hello, playground"
+greeting += " S"
+greeting += "w"
+greeting += "i"
+greeting += "f"
+greeting += "t"
+greeting += "!"
+print(greeting)
+
+let bye = greeting.replacingOccurrences(of: "Hello", with: "Bye")
+print(bye)
```

- '@@' 표시는 단순히 Hunk Range Information을 표시하기 위한 구분자이다.
- Hunk Range Information은 '-1,3', '+1,13' 이런식으로 두가지 범위를 포함한다.
- 첫번째 범위는 원본 파일에 대한 Hunk 범위 정보를 나타낸다. 즉, 이전 커밋의 Hunk 범위 정보이다.
- 두번째 범위는 변경된 파일에 대한 Hunk 범위 정보를 나타낸다.
- '-1,3'의 '-'는 원본 파일, '+1,13' 의 '+'은 변경된 파일을 의미한다.
- '-1,3'의 1은 Hunk의 시작 라인 번호를 의미하고, 3은 Hunk의 총 라인 수를 의미한다.
  ```swift
  1 import UIKit
  2
  3 var greeting = "Hello, playground"
  ```
- '+1,13'의 1도 마찬가지로 Hunk의 시작 라인 번호를 의미하고, 13은 변경된 파일 Hunk의 총 라인 수를 의미한다. 다만 이전 커밋엔 3개의 라인이 있었는데, 새로운 변경 사항이 반영되어 13개의 라인이 된것이다.
  ```swift
  1 import Foundation
  2
  3 var greeting = "Hello, playground"
  4 greeting += " S"
  5 greeting += "w"
  6 greeting += "i"
  7 greeting += "f"
  8 greeting += "t"
  9 greeting += "!"
  10 print(greeting)
  11
  12 let bye = greeting.replacingOccurrences(of: "Hello", with: "Bye")
  13 print(bye)
  ```

위 변경사항을 일단 커밋하고 아래와 같이 코드를 변경한다.

```swift
import Foundation

var greeting = "Hello, playground"
greeting += "~"
greeting += " S"
greeting += "w"
greeting += "i"
greeting += "f"
greeting += "t"
greeting += "!"
print(greeting)

var bye = greeting.replacingOccurrences(of: "Hello", with: "Bye")
bye += "!"
print(bye)
```

그리고 git diff를 하면 아래와 같은 결과가 나온다.
두개의 Hunk가 나타났다.

```
@@ -1,6 +1,7 @@
 import Foundation

 var greeting = "Hello, playground"
+greeting += "~"
 greeting += " S"
 greeting += "w"
 greeting += "i"

@@ -9,5 +10,6 @@ greeting += "t"
 greeting += "!"
 print(greeting)

-let bye = greeting.replacingOccurrences(of: "Hello", with: "Bye")
+var bye = greeting.replacingOccurrences(of: "Hello", with: "Bye")
+bye += "!"
 print(bye)
```

- 첫번째 Hunk는 중간에 "greeting += "~""만 추가되었으므로 총 코드 라인수만 6줄에서 7줄이 되었다.
- 두번째 Hunk는 총 코드 라인수 뿐만 아니라, 시작 라인 번호가 9에서 10으로 바뀌었다. 첫번째 Hunk에서 하나의 라인이 추가되어서 두번째 Hunk의 시작 라인 번호가 하나 더 늘어난 것이다.
- 만약 첫번째 Hunk에서 2개의 라인이 삭제되었다면, 두번째 Hunk의 시작 라인번호는 9에서 7이 되었을 것이다.
- 두번째 Hunk의 총 라인수는 한개가 삭제되고 두개가 추가되었으니깐 5개에서 6개가 되었다.

## 정리

- git diff에서 "@@ -1,3 +1,5 @@"는 Hunk의 범위 정보(Hunk Range Information)를 의미
- Hunk는 코드 변경사항이 일어난 부분
- Hunk 범위 정보엔 이전 파일의 범위와 최신 파일의 범위 두개의 범위 정보를 포함
- Hunk의 범위 정보에서만 예외적으로 "-"은 이전파일 "+"은 최신파일을 의미("-"가 삭제, "+"가 추가라는 의미가 아니라..)
- 두개의 숫자는 Hunk의 시작라인 번호, Hunk에 포함된 총 라인수를 의미
- @@ {이전 Hunk의 시작라인 번호}, {이전 Hunk에 포함된 총 라인수} {최신 Hunk의 시작라인 번호}, {최신 Hunk에 포함된 총 라인수} @@

## 참고

- [https://en.wikipedia.org/wiki/Diff#Unified_format](https://en.wikipedia.org/wiki/Diff#Unified_format)
- [https://www.gnu.org/software/diffutils/manual/html_node/Hunks.html](https://www.gnu.org/software/diffutils/manual/html_node/Hunks.html)
- [https://stackoverflow.com/questions/10950412/what-does-1-1-mean-in-gits-diff-output/10950496](https://stackoverflow.com/questions/10950412/what-does-1-1-mean-in-gits-diff-output/10950496)
