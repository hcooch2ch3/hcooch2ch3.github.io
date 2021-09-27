---
layout: post
title: ReactiveX와 RxSwift [초안]
tags: 
- ReactiveX
- RxSwift
---

요즘 MVVM을 공부하면서 RxSwift를 사용하는 예제를 접하게 되었다. 또한 현업에서 RxSwift를 많이 사용한다는 얘기를 많이 듣기도 했고, 취업 공고만 보더라도 RxSwift를 우대사항에 적어놓은 기업을 많이 보곤 하였다.
그래서 이 참에 RxSwift부터 공부해보고자 글을 쓰게 되었다.

## ReactiveX(반응형 프로그래밍)이란?

[ReactiveX 공식 홈페이지](http://reactivex.io/)의 첫 화면에는 ReactiveX에 대한 간단 명료한 정의가 다음과 같이 적혀있다.

> An API for asynchronous programming with observable streams

너무나 간단한 설명이라서, 처음엔 비동기 프로그래밍을 위한 API 정도라고만 이해할 수 있었다.
그리고 그 아래에는 다음과 같은 설명이 적혀있다. 

> ReactiveX is a combination of the best ideas from the Observer pattern, the Iterator pattern, and functional programming

ReactiveX는 Observer 패턴과 Iterator 패턴 그리고 함수형 프로그래밍을 이용하여 만든 것이라고 한다.

정리해보면, ReactiveX는 관찰 가능한 데이터 흐름을 비동기 프로그래밍 하기 위한 API 인데, Observer pattern, the Iterator pattern, 그리고 함수형 프로그래밍을 이용하여 구현이 되었다고 한다.  

썩 와닿지는 않지만, 어쨋든 ReactiveX를 이해하기 위해선 먼저 **함수형 프로그래밍**과 **동시성(비동기) 프로그래밍**을 알아야 하고, 디자인 패턴중에서는 **Observer Pattern**과 **Iterator Pattern**을 알아야 한다는 것을 알 수 있었다. 그래서 사람들이 ReactiveX의 Learning Curve가 높다고 하나보다. 

이보다 더 쉬운 설명을 찾다가 얄코님의 ['반응형 프로그래밍이 뭔가요? (+ ReactiveX 강좌)'](https://youtu.be/KDiE5qQ3bZI)를 보게 되었다.

- Observable (Stream) : 시간의 흐름, 사용자의 동작, 네트워크 요청의 결과
- Operator : filter, take, map 등 (순수함수 형태), 데이터를 정제
- Observer (Subscribe)

Observer가 발행물에 반응한다. 이것을 구독한다라고 한다.
Observable가 연속적으로 발행하는 연속된 값들의 흐름을 Stream이라고 한다.
스트림으로부터 나오는 값들에 구독자가 '반응'해서 특정 작업들을 처리하기 때문에 반응형 프로그래밍라고 부른다고 한다.


## 참고
- 반응형 프로그래밍이 뭔가요? (+ ReactiveX 강좌) : [https://youtu.be/KDiE5qQ3bZI](https://youtu.be/KDiE5qQ3bZI)
- RxSwift 4시간에 끝내기 (종합편) : [https://youtu.be/w5Qmie-GbiA?list=PLYBpLzrmzdNR_eN4s9xthaDP6HYEBeLaj](https://youtu.be/w5Qmie-GbiA?list=PLYBpLzrmzdNR_eN4s9xthaDP6HYEBeLaj)
- ReactiveX 공식 홈페이지 : [http://reactivex.io/](http://reactivex.io/)
