---
layout: post
title: DispatchQueue  serial/concurrent VS sync/async
tags: 
- dispatch-queue
---

DispatchQueue를 사용하면서 serial/concurrent VS sync/async 의 차이가 무엇인지 정확히 모르고 사용했었다.

일단 모르고 사용한다는 것이 부끄럽고, 이를 몰라서 큰 실수를 해버렸기에 정리해보고자 한다.

serial/concurrent는 DispatchQueue 안의 작업들이 어떤 방식으로 실행될건지에 대한 개념이다.

serial은 작업을 순차적으로 실행하는 방식이다.

concurrent는 작업을 병행적으로 실행하는 방식이다. 

예를 들어 내가 현재 통화, 운전, 네비게이션 검색 3가지 작업을 해야한다고 가정해보면,

serial 방식으로 이를 진행할 경우, 일단 통화를 하면서 장소를 묻는다. 통화가 다 끝나면 네비게이션을 검색해서 목적지를 설정한다. 그리고 목적지 설정이 끝나면 운전을 시작한다. 이처럼 작업을 순차적으로 진행하는 것이다. 

concurrent 방식으로 이를 진행할 경우, 오른손으론 네비게이션을 검색하면서 동시에 어깨에 핸드폰을 끼고 통화를 하면서 왼손으론 운전을 하는 방식으로 3가지를 동시에 진행하는 것이다. 

sync/async는 DispatchQueue에서 진행되는 작업들을 완료될때 까지 기다릴건지, 기다리지 않을건지에 대한 개념이다.

sync는 DispatchQueue 안의 작업이 끝날 때까지 외부에서 기다린 다음에 끝나면 큐 외부의 작업을 진행하는 것이다. 

async는 DispatchQueue 안의 작업이 진행되는 동안 기다리지 않고 큐 외부의 작업을 동시에 진행하는 것이다.  

예를 들어, 내가 현재 운전, 통화, 네비게이션 검색 3가지 작업을 해야한다고 가정하고, 세미나에 참석해야 하는 상황으로 가정해보면,

sync 방식을 채택할 경우, 내가 3가지 작업을 모두 완료하여 세미나 장소에 도착을 할 경우, 세미나를 시작하게 된다.

async 방식을 채택할 경우, 내가 3가지 작업을 완료하지 못해서 아직 세미나 장소에 도착하지 않아도, 이를 기다리지 않고 세미나를 진행하는 것이다.

정리해보면, 
serial/concurrent은 DispatchQueue의 내부 작업들을 순차적으로 진행할 것인지 병행적으로 진행할 것인지에 대한 개념이다.
sync/async는 DispatchQueue의 내부 작업이 진행되는 동안 외부에서 이를 완료될때까지 기다릴 것이냐, 기다리지 않고 외부의 작업을 진행할 것이냐에 대한 개념이다.

참조: [zeddios 블로그](https://zeddios.tistory.com/516)
