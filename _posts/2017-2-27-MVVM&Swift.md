# Planday Code Structure Guide.

## MVVM & RxSwift

Why RxSwift

- Why does writing async code have to be a nightmare? Functional reactive programming aims to neat-ify your async woes by giving you the power to operate on closures the same way you operate on variables. RxSwift is a brand new library that aims to make your event-driven apps incredibly manageable and readable, all while reducing bugs and headaches. Max Alexander shows you the basics, and how functional reactive programming can do all this and more.
[Functional Reactive Programming with RxSwift](https://realm.io/news/slug-max-alexander-functional-reactive-rxswift/)

- There are so many ways that objects can talk to each other in an iOS App: delegates, callbacks, notification. You can easily streamline your development process in 3 easy patterns with RxSwift. 
[Why RxSwift](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/Why.md)


Why MVVM 

- MVVM solves the MVC pattern (everyone jokes about the “Massive View Controller”). It seems to be the case that many engineers start with a view controller (it feels good: it’s already bootstrapped for you when you start a new project). You shove everything in there. And you don’t stop. You don’t stop for years, and then you end up with legacy apps...

Instead of shoving everything into your view controller, we’re going to do a Model, View, and ViewModel.
[MVVM with RxSwift](https://realm.io/news/slug-max-alexander-mvvm-rxswift/)



Learn in Action
======
[RxTodo code](https://bitbucket.org/planday/ios-style-guide/src/b0689d83097c4771b525a6ee16afc2ad50639c1e/RxTodo/?at=master)

RxTodo is an iOS application developed using [RxSwift](https://github.com/ReactiveX/RxSwift) and MVVM design pattern. This project is for whom having trouble with learning RxSwift and MVVM due to lack of references. (as I did 😁)


Features
--------

* MVVM design pattern
* Using [RxDataSources](https://github.com/RxSwiftCommunity/RxDataSources)
* Observing model create/update/delete across the view controllers
* Navigating between view controllers
* Immutable models
* Testing with [RxExpect](https://github.com/devxoul/RxExpect)


Philosophy
----------

* View doesn't have control flow. View cannot modify the data. View only knows how to map the data.

**Bad**

```swift
viewModel.titleLabelText
.map { $0 + "!" } // Bad: View should not modify the data
.bindTo(self.titleLabel)
```

**Good**

```swift
viewModel.titleLabelText
.bindTo(self.titleLabel.rx.text)
```

* View doesn't know what ViewModel does. View can only communicate to ViewModel about what View did.

**Bad**

```swift
viewModel.login() // Bad: View should not know what ViewModel does (login)
```

**Good**

```swift
self.loginButton.rx.tap
.bindTo(viewModel.loginButtonDidTap) // "Hey I clicked the login button"

self.usernameInput.rx.controlEvent(.editingDidEndOnExit)
.bindTo(viewModel.usernameInputDidReturn) // "Hey I tapped the return on username input"
```

* Model is hidden by ViewModel. ViewModel only exposes the minimum data so that View can render.

**Bad**

```swift
struct ProductViewModel {
let product: Driver<Product> // Bad: ViewModel should hide Model
}
```

**Good**

```swift
struct ProductViewModel {
let productName: Driver<String>
let formattedPrice: Driver<String>
let formattedOriginalPrice: Driver<String>
let isOriginalPriceHidden: Driver<Bool>
}
```


Requirements
------------

* iOS 8+
* Swift 3
* CocoaPods


Screenshots
-----------

![rxtodo](https://cloud.githubusercontent.com/assets/931655/21965942/1611927a-dbad-11e6-99ee-3509d06dc242.png)

