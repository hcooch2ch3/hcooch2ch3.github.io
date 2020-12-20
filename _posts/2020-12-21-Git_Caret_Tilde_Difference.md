---
layout: post
title: Git ~(caret) VS ^(tilde)
tags: 
- git
---

Git에서 reset, checkout 등을 사용하다보면, HEAD~, HEAD^ 을 자주 사용하게 된다.

^, ~의 차이가 뭘까 궁금해서 이를 공부하고 정리해보고자 한다.

* HEAD~n : HEAD의 n번째 조상을 의미한다. 즉, (((첫번째 부모)의 첫번째 부모)의 첫번째 부모)을 n번 재귀함을 의미한다. 예를 들어, 나~3은 나의 부모의 부모의 부모이므로 증조부모이다.

* HEAD^n : HEAD의 부모가 여러명일때, 그 중에서 n번째 부모를 의미한다. 예를 들어, 나를 낳아준 부모와 키워준 부모 이렇게 2부모가 있을 경우, 나^1 은 낳아준 부모, 나^2은 키워준 부모이다.

### 예시

(1) 아래의 상태에서 checkout으로 HEAD를 옮겨보려고 한다.
```
*   b222df7bca00363c0da5acb34665e5c77d098e64 (HEAD -> branch3) merge
|\  
| * a646e3a12eb6b0c5bb9798d6148f2ef6a6433f11 (branch2) second2 commit
* | 3650d73e8676fe2149590ac7121ddea7bc2a5463 third commit
* | f1f7922730532503f0f6367f4ab3a4b4fbef070e second3 commit
|/  
| * 72f2e5c31805860709566388597626f7efe5dd11 (master) second commit
|/  
* 038d37438283781b78ca152b4fe9c20a344ed620 first commit
```

(2) "git checkout HEAD~2"을 입력했더니 아래와 같이 부모의 부모로 HEAD를 이동하였다.
```
*   b222df7bca00363c0da5acb34665e5c77d098e64 (branch3) merge
|\  
| * a646e3a12eb6b0c5bb9798d6148f2ef6a6433f11 (branch2) second2 commit
* | 3650d73e8676fe2149590ac7121ddea7bc2a5463 third commit
* | f1f7922730532503f0f6367f4ab3a4b4fbef070e (HEAD) second3 commit
|/  
| * 72f2e5c31805860709566388597626f7efe5dd11 (master) second commit
|/  
* 038d37438283781b78ca152b4fe9c20a344ed620 first commit
```

(2') "git checkout HEAD~2"을 입력하면 아래와 같이 2번째 부모로 HEAD를 이동하게 된다.
```
*   b222df7bca00363c0da5acb34665e5c77d098e64 (branch3) merge
|\  
| * a646e3a12eb6b0c5bb9798d6148f2ef6a6433f11 (HEAD, branch2) second2 commit
* | 3650d73e8676fe2149590ac7121ddea7bc2a5463 third commit
* | f1f7922730532503f0f6367f4ab3a4b4fbef070e second3 commit
|/  
| * 72f2e5c31805860709566388597626f7efe5dd11 (master) second commit
|/  
* 038d37438283781b78ca152b4fe9c20a344ed620 first commit
```

P.S. HEAD^^와 HEAD^2는 다르다. HEAD^^ == (HEAD^)^ != HEAD^2 
