---
layout: post
title: stub vs mock
tags: 
- stub
- mock
---

얼마 전, 서비스 레이어에 대한 테스트 코드를 짰는데 시니어 개발자 지민께서 피드백을 주셨다.
"내가 mock이라고 짠 코드는 사실 mock이 아니라 stub이라고"
stub이 뭘까? 그럼 내가 잘못 알고 있는 mock은 원래 무엇일까? 라는 의문이 들었다.
이 참에 정리해보면서 확실히 알아가고자 한다.

Repository로부터 받아온 데이터를 받아서 서비스 레이어가 동작한다.


## 참고
- Mocks Aren't Stubs : [https://martinfowler.com/articles/mocksArentStubs.html](https://martinfowler.com/articles/mocksArentStubs.html)
- What's the difference between a mock & stub? : [https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub](https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub) 
