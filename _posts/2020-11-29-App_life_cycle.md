---
layout: post
title: App's(or Scene) Life Cycle 
tags: 
- App
- Scene
- Life Cycle
---

App(또는 Scene)은 View Controller와 마찬가지로 Life Cycle이 있다.

View Controller의 Life cycle은 'View가 화면에 나타나고 사라지고'를 관리하기 위해서 있는 것이라면,

App(또는 Scene)의 Life Cycle은 'App이 Foreground로 전환하거나 Background로 전환 되는 것'을 관리하기 위해서 있는 것이다.

App의 Life Cycle이 아닌, App 또는 Scene이라고 하는 이유는 iOS 13부터 Scene 기능을 사용하면 App이 아닌 Scene의 Life Cycle을 관리 해야하기 때문이다.
(Scene은 App의 UI이며, 하나의 App은 여러개의 Scene을 포함할 수 있다.)

App은 다음과 같은 상태가 가능하다:
- Not Running: 앱 실행 전 상태.
- Inactive: 앱이 실행중에 있지만 일시정지한 상태. 
- Active: 앱이 실행중인 상태.
- Background: 앱이 백그라운드에서 최소한의 자원을 갖고 있으며 최소한의 실행이 가능한 상태.
- Suspended: 앱이 백그라운드에 진입 후에 아무것도 실행하지 않는 상태.

<img src="https://docs-assets.developer.apple.com/published/c63cd35863/4d403429-fa30-4706-863f-5e3617ee21d0.png"/>
<br>


Scene은 다음과 같은 상태가 가능하다:
- Unattached: Scene이 실행 전 상태
- Foreground Inactive: Scene이 실행 중에 있지만, 일시정지한 상태 
- Foreground Active: Scene이 실행중인 상태.
- Background: Scene이 백그라운드에서 최소한의 자원을 갖고 있으며 최소한의 실행이 가능한 상태.
- Suspended: Scene이 백그라운드에 진입 후에 아무것도 실행하지 않는 상태.

<img src="https://docs-assets.developer.apple.com/published/61283402a3/024b99c5-4ab6-4ee0-bb41-6e6426ec6a64.png"/>
<br>

App과 Scene의 상태는 이름만 조금 다르고 거의 동일해 보이지만, 일단 상태의 단위가 다르고(App이냐 Scene이냐), 각각의 상태에서 다른 상태로의 변화 가능 여부가 약간 다르다. (App은 Suspended에서 Inactive로의 상태변화가 가능하지만 Scene은 불가능하다 등...)



참조/이미지 출처: [애플 개발자 문서](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)


