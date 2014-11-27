---
published: false
---

## First impressions of Swift and iOS development

I started learning Swift recently in order to pick up some iOS programming and I'm pretty impressed with the language so far.

I'm really glad I came to iOS development after Swift was released because going through tutorials like [this one](http://www.raywenderlich.com/74904/swift-tutorial-part-2-simple-ios-app) made me realized how much of a pain in the ass Objective-C is.

I've done some kernel programming for an OS class in C and I already knew of all the baggage it carried so I'm super glad I don't have to go through that for iOS. I think the biggest thing iOS has going for it, is the tooling.

### Xcode

Xcode is a fantastic IDE. I'm sure every IDE has its performance problems (and I've heard of devs having Xcode crashing issues due to the Swift compiler) but having [used](http://www.jetbrains.com/webstorm/) a [bunch](http://eclipse.org) of [them](http://visualstudio.com), I can definitely put Xcode up there with Visual Studio and Webstorm. The UX is great, and far ahead of [ADT](http://developer.android.com/tools/sdk/eclipse-adt.html) on Eclipse (I haven't tried Android studio yet).

I also really love the systematic Model, View, ViewController way of developing an iOS apps. It reminds me of your typical MVC/MVVM family of architectures and any developer who has worked in those will feel right at home.

One of the biggest things I really like, is connecting View elements from the Interface Builder directly to your ViewController and how it creates the `@IBOutlet` property for you.

### Language syntax and features

Speaking of properties, I'm incredibly happy to see C# like properties as "computed properties" in Swift. Java's `getters` and `setters` are a thing of the past!

```
var subtotal: Double {
  get {
    return total / (taxPct + 1)
  }
}
```
Syntactic sugar like no need for parentheses on conditional statements is amazing and even though braces are required for single line conditional clauses, I'm glad that's enforced.


