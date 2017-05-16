---
layout: post
title: "Protocol Oriented Programming in Swift"
og_image_url: "https://nemanjavlahovic.github.io/res/wasm/poster.png"
description: "Explore how you can write flexible and extensible code in Swift with protocol-oriented programming."
figma_file: a2r9Ll65Et0eqCRVSwQVL25B
---

<img src="/res/wasm/swift-og.png" width="30%" align="right" alt="This is not a logotype">[Protocols](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html) are a very powerful feature of the Swift programming language.
You use them to define blueprint of methods, properties and other requirements that suit a particular task or piece of functionality. Any object that satisfies the requirements of a protocol is said to conform to that protocol.
Protocol oriented programming has been introduced as a new programming pattern with Swift 2.0 on [WWDC 2015.](https://www.youtube.com/watch?v=g2LwFZatfTI)

Before protocols became “the thing”, it was impossible to imagine iOS app that hasn’t implemented OOP paradigm. Just think about UIKit and its inheritance.
Although OOP is nice, there are few problems:
- You start with a single class, but at some point it turns out that you need a certain functionality from another class. Since multiple inheritance is not possible neither in Swift nor Objective C, this causes headache. (Multiple inheritance is supported in C++, but that isn’t much of a use here).
- When working in a multi-thread environment, passing around references of classes can cause more trouble than initially hoped for.
- Tight coupling and lack of testability.

Internet is full of rants about OOP like [this.](https://krakendev.io/blog/subclassing-can-suck-and-heres-why)

### Introduction to protocol-oriented programming

Let’s say we have a Vehicle, which we’ll define as a protocol that will have one property:
```wasm
protocol Vehicle {
  var hitPoints: Int {get set} 
}
```

Using protocol extensions we can extend the functionality and provide additional methods to the Vehicle protocol:
```wasm
extension Vehicle {
  mutating func takeHit(amount: Int) { hitPoints -= amount } 
  func hitPointsRemaining() -> Int { return hitPoints }
  func isAlive() -> Bool { return hitPoints > 0 ? true : false }
}
```

Now, let’s define LandVehicle, SeaVehicle and AirVehicle protocols:
```wasm
protocol LandVehicle: Vehicle { 
  var landAttack: Bool {get}
  var landMovement: Bool {get} 
  var landAttackRange: Int {get}
  func doLandAttack()
  func doLandMovement() 
  }
  
protocol SeaVehicle: Vehicle { 
  var seaAttack: Bool {get}
  var seaMovement: Bool {get} 
  var seaAttackRange: Int {get}
  func doSeaAttack()
  func doSeaMovement() 
}
protocol AirVehicle: Vehicle { 
  var airAttack: Bool {get}
  var airMovement: Bool {get} 
  var airAttackRange: Int {get}
  func doAirAttack()
  func doAirMovement() 
}
```

Important thing to note is that all 3 protocols inherit from Vehicle protocol.

Now, let’s define a struct that conforms to Vehicle protocol:

struct Helicopter: AirVehicle {
	var hitPoints = 100
	let airAttackRange = 5
	let airAttack = true
	let airMovement = true

	func doAirAttack() { print(“Helicopter Attack”) }
	func doAirMovement() { print(“Helicopter movement”) }
}

But what if we had a vehicle that could do both air, land & sea attacks? 🤔 Say no more:

```wasm
struct Transformer: LandVehicle, SeaVehicle, AirVehicle {
  var hitPoints = 75
  let landAttackRange = 7 
  let landAttack = true 
  let landMovement = true 
  let seaAttack = true 
  let seaMovement = true 
  let airAttack = true 
  let airMovement = true
  func doLandAttack() { print("Transformer Land Attack") } 
  func doLandMovement() { print("Transformer Land Move") } 
  func doSeaAttack() { print("Transformer Sea Attack") } 
  func doSeaMovement() { print("Transformer Sea Move") } 
  func doAirAttack() { print("Transformer Sea Attack") } 
  func doAirMovement() { print("Transformer Sea Move") }
}
```

### Summary

This way we have achieved something similar to multiple inheritance, but in a very elegant way, without OOP paradigms.
We have used protocol inheritance and composition to create protocols with very specific requirements, which allowed us to create abstract types.
You should be aware, though, that this isn’t a silver bullet solution and you shouldn’t use POP in every possible scenario just because it is a funky new programming paradigm.
