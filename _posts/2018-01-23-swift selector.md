---
layout: post
title: "Extending Swift Selector"
og_image_url: ""
description: "Pragmatic solution for Selectors readability"
---

After that throwback post about ancient Objective-C, time to dive in back to Swift.

I can't remember if anything was changed more often than Selectors in Swift. In Swift 2.0, you were able to write the selector as a String (i.e "buttonClicked"). Main problem with this approach is that the compiler can't check whether the method exists or not at compile time. No need to further explain how error prone this is.

However, later versions of Swift brought us some improvements, and deprecated stringified approach to selectors. In Swift 4, adding selector would look like this:
```wasm
NotificationCenter.default.addObserver(self, selector: #selector(NotificationObserver.notificationHandler(_:)), name:NSNotification.Name, object: self.object)
```

Not sure how other people feel about it, but I always had hard time reading that selector syntax. Fortunately, there is a workaround.
```wasm
private extension Selector {
    static let notificationHandler = #selector(NotificationObserver.notificationHandler(_:))
}

...
NotificationCenter.default.addObserver(self, selector: .notificationHandler, name:NSNotification.Name, object: self.object)
```

Writing an extension for Selector lets you add static constant of the selector we want to use.
Convenient, right?
