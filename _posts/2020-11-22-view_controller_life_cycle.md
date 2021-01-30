---
layout: post
title: View Controller Life Cycle
tags: 
- view-controller
- life-cycle
---

View Controller는 View와 Model 중간에서 매개하는 역할을 하며 View를 관리한다.

View가 로드되고 해제되는 것 등 View와 관련된 것들을 View Controller가 관리하기 때문에, View가 나타나고 사라지는 과정에 따라 View Controller의 상태가 결정된다.

즉, View Controller Life Cycle은 View가 로드되서 화면에 나타나고 사라지는 과정에 따라 가능한 뷰 View Controller의 4가지 상태와 상태 변화에 관한 것이다.

View Controller는 총 4가지 상태가 가능하다.
1. Appearing(나타나는 중)
2. Appeared(나타남)
3. Disappearing(사라지는 중)
4. Disappeared(사라짐)

그리고 iOS는 View Controller 상태변화 중간에 View Controller에 구현된 method를 호출한다.
- viewDidLoad() : 뷰가 생성되서 로드된 직후에 호출된다.
- viewWillAppear() : 뷰가 나타나기 직전(앱의 뷰 계층구조에 뷰가 추가되기 직전)에 호출된다.
- viewDidAppear() : 뷰가 나타난 직후(앱의 뷰 계층구조에 뷰가 추가된 직후)에 호출된다.
- viewWillDisappear() : 뷰가 사라지기 직전(앱의 뷰 계층구조에서 뷰가 삭제되기 직전)에 호출된다. 
- viewDidDisappear() :뷰가 사라진 직후(앱의 뷰 계층구조에서 뷰가 삭제된 직후)에 호출된다.

필요에 맞게 위 method를 View Controller에 구현하면 된다. 

참조 : [애플 개발자 문서](https://developer.apple.com/library/archive/referencelibrary/GettingStarted/DevelopiOSAppsSwift/WorkWithViewControllers.html)
