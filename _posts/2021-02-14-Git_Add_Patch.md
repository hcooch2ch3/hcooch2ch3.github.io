---
layout: post
title: git add -p(--patch)
tags: 
- git
---

아직은 초보개발자라서 프로젝트를 진행하면서 커밋을 관리하기가 어려웠다. 정신없이 코드를 짜다가 커밋하는것을 잊어버리기 일쑤였고, 결국은 매우 큰 단위로 커밋해버리곤 하였다. 그러다가 git add의 -p 옵션을 알게 되었고, 이 옵션을 이용하여 커밋을 잘 나눌 수 있게 되었다. (git add --patch에 대하여 공유해준 오동나무에게 감사합니다.)

## git add -p란?
git add -p은 수정된 코드를 잘게 나누어서 add할 수 있도록 하는 옵션이다. (-p는 patch을 의미하며, patch는 사전상의 의미로 '부분', '조각'을 의미한다. ) git add는 1개의 파일씩 add가 가능하지만 -p 옵션을 통해 1개의 파일 내부에서도 부분을 나누어서 add를 할 수 있다.

아래와 같이 Dog.swift 파일을 만들어서 구현을 하고, add를 하려는 상황이다.
```
// Dog.swift

import Foundation

class Dog {
    
    func eat() {
        
    }
    
    func bark() {
        
    }
    
}
```
'git add Dog.swift' 를 입력하면 다음과 같이 코드 전체가 add 된다.

![git add Dog.swift](/assets/images/git_add.png)

하지만 -p 옵션을 이용하면 아래와 같이 eat 함수만 add 할 수 있다. (아래 사진은 git add -p 과정은 포함 하지 않고, 이후의 status만 출력한 것이다.)

![git add -p Dog.swift](/assets/images/git_add_patch.png)


## git add -p는 어떻게 사용할까?
1. 아래 사진 처럼 git add -p를 입력하면 git은 hunk를 차례대로 출력한다. (hunk는 수정된 코드의 일부를 의미하는 용어이며, git은 자동으로 hunk를 나누어 생성한다.) 
2. 그리고 "(1/1) Stage this hunk [y,n,q,a,d,e,?]?" 라는 메세지와 함께 hunk를 add할 것인지 아닌지 묻는다. 
3. 우리는 원하는 선택지를 입력하면 된다. [y,n,q,a,d,e,?] 은 선택지이며, y를 입력하면 hunk가 add된다. 


![git add -p Dog.swift](/assets/images/git_add_patch_hunk.png)

## 다양한 Hunk 선택지
선택지에 대한 자세한 설명은 ?를 입력하면 나온다. 위 사진은 [y,n,q,a,d,e,?]  총 7개의  선택지만 가능하지만, git은 자동으로 판단하여 현재 선택 가능한 선택지를 출력한다. 때에 따라서 's', 'j', 'k' 등이 선택 가능할 때도 있다. 아래는 모든 선택지에 대한 간단한 설명이다.

- y - stage this hunk
- n - do not stage this hunk
- q - quit; do not stage this hunk or any of the remaining ones
- a - stage this hunk and all later hunks in the file
- d - do not stage this hunk or any of the later hunks in the file
- g - select a hunk to go to
- / - search for a hunk matching the given regex
- j - leave this hunk undecided, see next undecided hunk
- J - leave this hunk undecided, see next hunk
- k - leave this hunk undecided, see previous undecided hunk
- K - leave this hunk undecided, see previous hunk
- s - split the current hunk into smaller hunks
- e - manually edit the current hunk
- ? - print help


## Git이 Hunk를 나누는 기준은?
Git은 hunk를 자동으로 나누어 준다. Git은 새로운 변동사항 사이에 기존 내용이 7줄 이상 있을때 새로운 hunk로 나눈다.
위에서 구현한 Dog class에 walk 메서드와 sleep 메서드를 새로 추가하면 아래와 같다.
```
import Foundation

class Dog {
    
    func walk() {
        // 새로 구현
    }
    
    func eat() {
        // 기존 내용
    }
    
    func bark() {
        // 기존 내용
    }
    
    func sleep() {
        // 새로 구현
    }
    
}
```
그리고 git add -p하면 아래의 사진과 같이 walk와 sleep은 다른 hunk로 분리된 것을 확인할 수 있다.
두개의 hunk가 분리된 이유는 walk와 sleep 사이가 기존의 내용(eat 메서드, bark 메서드)이 7줄 이상 있었기 때문이다. 

![git add -p Dog.swift 2 hunk](/assets/images/git_add_patch_2hunk.png)

만약 sleep을 eat 메서드 위에 구현하면 아래의 사진과 같이 walk와 sleep은 한개의 hunk에 포함된다. walk와 sleep 사이에 기존의 내용이 eat 함수 3줄(공백 포함 4줄) 뿐이기 때문이다. 즉, 새로운 내용 사이에 기존의 내용이 7줄 이상이 되지 않기 때문이다. 

![git add -p Dog.swift 1 hunk](/assets/images/git_add_patch_1hunk.png)

## hunk보다 더 작은 단위로 add 하는방법

선택지 's'와 'e'를 이용하면 Git이 자동 생성한 Hunk보다 더 작은 단위로 add 할 수 있다. 's'는 현재 Hunk보다 더 작은 Hunk를 자동으로 나누어서 생성해준다. 하지만 's'는 Git이 자동으로 판단하여 나눠준다는 점 때문에 약간의 불편함이 있으며, 특정 조건을 만족해야만 's' 선택지를 선택할 수 있다는 불편함이 있다. 그래서 더 디테일하게 add할 내용을 선택하려면 'e'를 선택하면 된다. 'e'는 현재 Hunk에서 add할 내용과 add 하지 않을 내용을 직접 한줄 한줄 선택할 수 있도록 해준다. 또한 'e'는 's'처럼 특정 조건에서만 선택할 수 있는 것이 아니라 모든 조건에서 선택할 수 있다. 

## 's' 는 어떤 조건에서 선택 가능하며 어떻게 hunk를 나눌까?

's'는 hunk의 새로운 변동사항 사이에 7줄 이하의 기존 내용이 있으면 선택이 가능하다(7줄 이상이면 다른 hunk로 자동으로 나눠지므로). 그리고 이 때 's'를 선택하면 Git은 기존의 내용을 기준으로 hunk를 둘로 나누어 준다.

아래의 코드 처럼 Dog class에서 eat, bark 메서드를 구현하여 add하고, 이후에 walk 메서드와 sleep 메서드를 새로 구현했다고 가정하자. sleep메서드는 eat 메서드와 bark 메서드 사이에 새롭게 구현하였다. 
```
import Foundation

class Dog {
    
    func walk() {
        // 새로 구현
    }
    
    func eat() {
        // 기존 내용
    }
    
    func sleep() {
        // 새로 구현
    }
    
    func bark() {
        // 기존 내용
    }
    
}
```
그리고 git add -p 한 후 's'를 선택하면, 아래의 사진과 같이 eat 메서드를 기준으로 hunk가 나눠지는 것을 확인할 수 있다. 

![git add -p Dog.swift split](/assets/images/git_add_patch_split.png)

## 'e'는 어떻게 사용할까?
'e'를 선택하면 vi 에디터 같은 편집창이 나타나면서 add할 내용과 아닌 내용을 직접 편집할 수 있다. 
1. 편집창에는 삭제된 코드 앞에 '-' 문자로 표시되고, 추가된 코드 앞에 '+' 문자로 표시된다.
2. 코드의 삭제와 추가를 모두 add 하려면 이 상태 그대로 저장하고 나가면 된다(esc 누르고 :wq 입력).
3. 만약 삭제된 코드를 add 하지 않으려면 '-' 문자만 삭제하면 된다. 
4. 그러나 추가된 코드를 add 하지 않으려면 해당 코드를 편집창에서 삭제해야한다(d를 두번 누른다).

위의 Dog class 코드에서 구현한 4개의 메서드를 add한 후, 아래의 코드처럼 bark 메서드를 삭제하고 speak 메서드를 새로 구현하였다고 가정하자.

```
import Foundation

class Dog {
    
    func walk() {
        // 기존 내용
    }
    
    func eat() {
        // 기존 내용
    }
    
    func sleep() {
        // 기존 내용
    }
    
    func speak() {
        // 새로 구현(bark 메서드는 삭제)
    }
    
}
```

그리고 git add -p 입력 후 e를 선택하면 아래와 같은 편집 창이 나타난다. 편집 창에는 bark 메서드가 삭제되었고, speak 메서드가 추가되었다고 한다.

![git add -p Dog.swift edit](/assets/images/git_add_patch_edit.png)

이를 그대로 반영하려면(add하려면) 저장하고 나가면 된다.

만약 bark 메서드의 삭제를 add 하지 않고 sepak 메서드의 구현만 add하려면 아래의 사진처럼 '-' 문자를 없애고 저장하면 된다.

![git add -p Dog.swift edit minus](/assets/images/git_add_patch_edit_minus.png)

speak 메서드의 구현을 add 하지 않고 bark 메서드의 삭제만 add 하려면, 아래의 사진처럼 편집창에서 해당 코드를 삭제하고 저장하면 된다.

![git add -p Dog.swift edit plus](/assets/images/git_add_patch_edit_plus.png)

이처럼  'e'를 통해 add할 코드와 하지 않을 코드를 한 줄씩 직접 편집하여, hunk 단위로 add 하는 불편함을 해결할 수 있다.
