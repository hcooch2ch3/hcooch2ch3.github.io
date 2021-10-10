---
layout: post
title: ReactiveX와 RxSwift [초안]
tags: 
- ReactiveX
- RxSwift
---

요즘 MVVM을 공부하면서 RxSwift를 사용하는 예제를 접하게 되었다. 또한 현업에서 RxSwift를 많이 사용한다는 얘기를 많이 듣기도 했고, 취업 공고만 보더라도 RxSwift를 우대사항에 적어놓은 기업이 많았다.
그래서 이 참에 RxSwift부터 공부해보고자 글을 쓰게 되었다.

## ReactiveX(반응형 프로그래밍)이란?

ReactiveX는 Reactive eXtensions의 줄임말이라고 한다. 마이크로소프트에서 개발하였으며 처음엔 .NET Framework에서 사용하기 위해 C#으로 구현하였다고 한다. 지금은 RxJava, RxJS 등 여러 언어에서 사용될 수 있도록 구현이 되었다고 하는데, 그 중 Swift에서 사용할 수 있도록 구현한 것이 RxSwift 이다.

[ReactiveX 공식 홈페이지](http://reactivex.io/)의 첫 화면에는 ReactiveX에 대한 간단 명료한 정의가 다음과 같이 적혀있다.

> An API for asynchronous programming with observable streams

너무나 간단한 설명이라서, 처음엔 비동기 프로그래밍을 위한 API 정도라고만 이해할 수 있었다.
그리고 그 아래에는 다음과 같은 설명이 적혀있다. 

> ReactiveX is a combination of the best ideas from the Observer pattern, the Iterator pattern, and functional programming

ReactiveX는 Observer 패턴과 Iterator 패턴 그리고 함수형 프로그래밍을 이용하여 만든 것이라고 한다.

정리해보면, ReactiveX는 관찰 가능한 데이터 흐름을 비동기 프로그래밍 하기 위한 API 이며, Observer 패턴, Iterator 패턴, 그리고 함수형 프로그래밍을 이용하여 구현이 되었다는 것이다.

썩 와닿지는 않지만.. 어쨋든 ReactiveX를 이해하기 위해 알아야 할 것은 다음  4가지이다. (이러니 Rx의 Learning Curve가 높다고 하나보다...)
- **비동기 프로그래밍**
- **함수형 프로그래밍**
- **Observer 패턴**
- **Iterator 패턴**

공식 홈페이지 보다 더 쉬운 설명을 찾다가 얄코님의 ['반응형 프로그래밍이 뭔가요? (+ ReactiveX 강좌)'](https://youtu.be/KDiE5qQ3bZI)를 보게 되었다.

**함수형 프로그래밍**이란 최대한 변수(상태) 사용을 자제하고 순수함수로만 프로그래밍하는 것을 의미한다. 순수함수 내부 구현은 변수를 사용하겠지만, 순수함수 외부에서 변수에 접근하거나 변경할 수 없기 때문에 더 안전하게 로직을 구현할 수 있게 된다고 한다. 즉, 프로그래머의 실수를 방지하고 멀티쓰레딩 환경에서 여러 스레드의 동시접근에 의한 오류 또는 교착문제를 방지 할 수 있다고 한다. 

옵저버블이 발행해서 파이프에서 정제된 값들을 구독자가 기다렸다가 그것들이 도착하는대로 지정한 작업들을 수행한다.
이와 같이 스트림으로부터 나오는 값들을 구독자가 '반응'해서 특정 작업들을 처리하기 때문에 '반응형' 프로그래밍이라고 한다.
데이터의 흐름을 옵저버블로 구현한 다음 이를 타고 나오는 데이터를 오퍼레이터로 정제해서 그 최종값에 구독자가 어떻게 반응할지를 코딩하는 것.

- Observable (Stream) : 시간의 흐름, 사용자의 동작, 네트워크 요청의 결과
- Operator : filter, take, map 등 (순수함수 형태), 데이터를 정제
- Observer (Subscribe)

Observer가 발행물에 반응한다. 이것을 구독한다라고 한다.
Observable가 연속적으로 발행하는 연속된 값들의 흐름을 Stream이라고 한다.
스트림으로부터 나오는 값들에 구독자가 '반응'해서 특정 작업들을 처리하기 때문에 반응형 프로그래밍라고 부른다고 한다.

## 참고
- ReactiveX: https://en.wikipedia.org/wiki/ReactiveX
- ReactiveX 공식 홈페이지 : [http://reactivex.io/](http://reactivex.io/)
- 반응형 프로그래밍이 뭔가요? (+ ReactiveX 강좌) : [https://youtu.be/KDiE5qQ3bZI](https://youtu.be/KDiE5qQ3bZI)
- RxSwift 4시간에 끝내기 (종합편) : [https://youtu.be/w5Qmie-GbiA?list=PLYBpLzrmzdNR_eN4s9xthaDP6HYEBeLaj](https://youtu.be/w5Qmie-GbiA?list=PLYBpLzrmzdNR_eN4s9xthaDP6HYEBeLaj)
