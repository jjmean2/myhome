---
layout: post
title: Nib 파일과 View Controller 생성
date:   2018-05-31 00:13:06 +0900
categories: mobile ios
tags: mobile ios nib viewController
---

UINib 객체는 nib 파일의 내용을 메모리에 캐시한다. Nib의 내용을 인스턴스화해야 할 때 UINib 객체가 있고 이전에 이미 한번 불러서 캐시가 있다면 Nib 파일을 로딩하는 절차를 생략하고 바로 할 수 있다. UINib 객체는 메모리가 부족할 때 자동으로 이 캐시를 비운다. 
TableViewCell을 반복적으로 인스턴스화하는 것과 같이 같은 Nib 데이터를 반복적으로 인스턴스화 할 일이 있으면 UINib 객체를 사용해 캐싱하는 것이 성능에 상당한 도움이 된다.

Nib 파일을 가지고 UINib 객체를 생성하면 이 객체는 Nib 파일에 기술된 객체 그래프를 로딩한다. 하지만 바로 unarchive하지는 않는다. (Nib이 객체 그래프 상태를 바이너리 형태로 압축한 것이라고 생각하면 이 데이터를 unarchive해야 메모리상에 인식 가능한 객체 그래프가 생긴다고 할 수 있다.) 이 archive된 Nib 데이터를 unarchive하기 위해서는 `instantiateWithOwner:options:` 메소드를 호출한다. 


컴퓨터 프로그램에서 리소스란 실행파일과 다른 것이다. 복잡한 데이터셋이나 그래픽 컨텐츠를 코드로 생성하는 것보다 더 쉬운 방법으로 생성하여 보관하는 것이다. 예를 들어 이미지를 픽셀하나씩 코드로 생성하는 것보다, 이미지 에디터로 생성하는 것이 더 효율적이다.

* Interface Object는 Nib이 실제로 인스턴스화하는 객체들이다. UIView 같은 것들을 말하는데 Interface Object로 추가할 수 있는 것은 View와 같은 visual object도 있지만, UIViewController나 Gesture Recognizer와 같이 non-visual object들도 있다.

* File's Owner
* First Responder

Nib을 로딩할 때 Nib파일에 기술한 모든 Object Graph를 생성한다. Top-Level Object는 이 중에서 Parent Object가 없는 Object들을 말한다. Nib으로 로딩한 객체들을 참조할 곳이 필요하다. UINib을 로딩할 때에 Nib과 별개로 생성한 객체를 Nib의 File's Owner 객체로 넘겨주는데 보통 이 File's Owner 내부에 IBOutlet으로 Nib의 객체들을 연결시키는 방식으로 참조를 만든다. 그렇게 하지 않는 경우 Nib을 로딩(인스턴스화)하는 메소드 (`instantiate(withOwner:options:)`)의 리턴값이 Top Level Objects이므로 그것을 사용한다.

앱은 보통 기본 Nib 파일이 있어서 앱이 실행될 때 이 Nib을 로딩한다. (요새는 스토리보드로 바뀌었지만) 실행시 로딩할 Main Nib을 어디서 찾아야 하는지는 앱의 기본설정에 기술하는데 `Info.plist`의 `NSMainNibFile` 키의 값으로 찾는다. 앱번들에서 그 값을 이름으로 가진 Nib을 찾아서 로딩한다. (extensions은 있어도 되고 없어도 된다.)

## UIViewController의 생성방법 (Nib, Storyboard, 코드)
UIViewController에 nib 파일명을 등록하면 그 ViewController의 view에 접근할 때 nib 파일이 자동으로 로딩된다. 이 때 ViewController와 Nib 파일의 객체 간의 연결도 이루어진다. UIViewController의 지정 초기화 메소드는 `init(nibName:bundle:)`이다. 이 때 nib 파일명을 인자로 전달한다. 
UIViewController의 view에 처음으로 접근할 때 view가 없으면(nil이면) `loadView()`를 호출하여 view를 생성한 후 그 결과를 반환한다고 한다. `loadView()` 메소드는 `nibName` 속성이 있다면 그 속성의 Nib 파일을 찾아서 로딩하고 Top Level View를 이 자기의 view로 설정할 것이다. 만약 `nibName`을 nil로 전달한다면 view를 구성할 때 nib을 쓰지 않을 것이다. 그럼 기본적인 `loadView()`에서 기본적인 view를 생성할 것으로 보인다.
UIViewController를 스토리보드로 만들 때는 Storyboard는 `init(nibName:bundle:)`으로 ViewController를 생성하지 않고 `init(coder:)`로 생성한다.

그럼 UIViewController를 스토리보드로 만들 때와 Nib으로 만들 때 그리고 코드로 만들 때의 차이를 알아보자.

### 스토리보드
스토리보드로 구성된 ViewController는 Storyboard의 Segue로 생성하거나 `UIStoryboard` 객체의 `instantiateViewController(withIdentifier:)` 메소드로 생성할 수 있다. 이 때 생성되는 ViewController는 `init(coder:)` 초기화 메소드가 호출되고 `nibName` 속성에는 Storyboad 안에 있는 nib파일이 할당된다고 한다. 그래서 테스트를 해 봤다. Storyboard로 만든 ViewController의 `nibName` 속성을 디버거에서 런타임에 찍어봤더니 다음과 같은 값이 나왔다.

```sh
(llvm) po self.nibName!
"BYZ-38-t0r-view-8bC-Xf-vdC"
```

### Nib
Nib으로 ViewController를 구성하면 그 ViewController를 생성할 때 보통 Nib을 직접 객체화시키지 않는다. Storyboard의 경우 `UIStoryboard` 객체를 가지고 객체화 시키는 데 반해 Nib의 경우 ViewController의 초기화 메소드인 `init(nibName:bundle:)`으로 ViewController를 생성하면서 `nibName` 인자로 nib 파일이름을 전달한다. 차이가 있는 이유는 Storyboard로 View Controller를 구성할 때는 View Controller 자체를 Nib에 포함시키는 반면, Nib으로 ViewController을 구성한다고 하면 보통 View Controller의 메인 View만 Nib으로 만들기 때문이다. ViewController를 코드로 생성시키고 nibName, nibBundle을 지정해 두면 그 ViewController의 view가 로딩될 때 (기본 `loadView()` 메소드에 구현되어 있을 듯) 그 nibBundle과 nibName으로 번들에서 nib 파일을 찾아서 로딩시키고 자기의 view로 설정하는 것이다. 만약 nibName이 nil이라면 다음의 과정을 순서대로 거쳐 Nib 파일을 찾는다. (앞의 방식에 우선순위가 있다.) (그래도 없으면 기본적인 UIView를 생성한다.)

1. view controller 클래스명이 `Controller`로 끝나면 `Controller`를 뺀 이름과 매칭되는 Nib 파일을 찾는다. 예를 들어 `MyViewController`라면 `MyView.nib` 파일을 찾는다.
2. view controller 클래스명과 같은 Nib 파일을 찾는다. 즉, `MyViewController`인데 `MyView.nib`이 없었다면 `MyViewController.nib`을 찾는다.

`Nib` 이름에 플랫폼 지정 identifier가 포함되어 있으면 그 플랫폼에서만 로딩된다. 
예를 들어 `MyViewController~ipad.nib` 이라면 ipad에서만 로딩된다. 플랫폼 지정 identifier는 `~iphone`, `~ipad`가 있다.

### 코드
코드로 생성할 때는 Nib으로 생성할 때와 마찬가지로 지정 초기화 메소드인 `init(nibName:bundle:)`을 사용해서 생성한다. Nib을 쓰는 방법도 ViewController는 코드로 생성하고 메인 View를 Nib으로 생성하는 것이었다. View도 코드로 생성하려면 `nibName`과 `nibBundle`을 nil로 준다. 그러면 적절한 Nib 파일이 없을 경우 메인 View는 가장 기본적인 UIView가 된다. 그리고 `viewDidLoad()`에서 원하는 UI Component를 직접 코드로 생성해서 집어넣는 것이다. Main View로 UIView가 아닌 것을 쓰려면 (ScrollView, TableView) `loadView()`를 오버라이드하여 원하는 뷰를 생성해서 `view`에 할당해야 한다.

---
Nib 을 로딩하는 방법으로 UINib 객체를 먼저 만들고 `instantiateWithOwner:options:` 메소드를 호출하는 방법이 있고 `Bundle` 객체에서 `loadNibNamed(nibName:options:)` 메소드를 호출하는 방법이 있다. 두 방법의 차이는 `UINib` 객체를 만들고 `loadNibNamed(nibName:options:)`를 호출하면 Nib 파일의 내용을 메모리에 불러오고 나서 그 데이터를 unarchive하여 실제 객체를 만든다. 그리고 UINib이 살아있는 동안 그 Nib 파일의 내용(archived된 버전)은 메모리에 캐시되어 있다. 반면 `Bundle`을 이용한 방법은 `loadNibNamed(nibName:options:)` 메소드를 호출할 때마다 Nib 파일을 디스크에서 읽는다. 즉, 중간에 캐싱의 과정이 없다. 따라서 `Nib`을 빈번하게 객체화시킬 필요가 있으면 UINib을 쓰는 것이 성능상 이점이 있다.

다음 주제: View Controller의 Size class (adapting), 여러 플랫폼 지원하기

# 참고자료
* http://zeddios.tistory.com/298
* http://suho.berlin/engineering/ios/ios-storyboard-nibxib-code/
* https://www.toptal.com/ios/ios-user-interfaces-storyboards-vs-nibs-vs-custom-code
* https://developer.apple.com/documentation/uikit/uinib#
* https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html#//apple_ref/doc/uid/10000051i
