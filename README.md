# Foundation

The Foundation framework defines a base layer of functionality that is required for almost all applications. It provides primitive classes and introduces several paradigms that define functionality not provided by either the Objective-C runtime and language or Swift standard library and language.

It is designed with these goals in mind:
* Provide a small set of basic utility classes.
* Make software development easier by introducing consistent conventions.
* Support internationalization and localization, to make software accessible to users around the world.
* Provide a level of OS independence, to enhance portability.

There is more information on the Foundation framework [here](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/ObjC_classic/).

> Important: This project is in the early stages of development. It is not yet ready for production use, but it is ready for contributions. It is scheduled to be part of the Swift 3 release.

This project provides an implementation of the Foundation API for platforms where there is no Objective-C runtime. On OS X, iOS, and other Apple platforms, apps should use the Foundation that comes with the operating system. Our goal is to abstract away the exact underlying platform as much as possible.

## Goals

### Part of Swift 3.0

Our primary goal is to achieve implementation parity with Foundation on Apple platforms. This will help to enable the overall Swift 3 goal of **portability**.

In our first year, we are not looking to make major API changes to the library. We feel that this will hamper the primary goal. There are some areas where API changes are unavoidable, however. For more information on those APIs and the overall design of Foundation, please see [our design document](Docs/Design.md).

### API Naming and Foundation

One of the goals of the Swift 3 project is [a new set of naming guidelines](https://swift.org/documentation/api-design-guidelines.html). The Foundation project will soon update all of its names to match the new guidelines. We will also drop the 'NS' prefix from all classes.

### Current Status

See our [status page](Docs/Status.md) for a detailed list of what features are currently implemented.

## Using Foundation

Here is a simple `main.swift` file which uses Foundation. This guide assumes you have already installed a version of the latest [Swift binary distribution](https://swift.org/download/#latest-development-snapshots).

```swift
import Foundation

// Make an URLComponents instance
let swifty = NSURLComponents(string: "https://swift.org")!

// Print something useful about the URL
print("\(swifty.host!)")

// Output: "swift.org"
```

You will want to use the [Swift Package Manager](https://swift.org/package-manager/) to build your Swift apps.

## Working on Foundation

Please see [Getting Started](Docs/GettingStarted.md).

## FAQ

##### Why include Foundation on Linux?

We believe that the Swift standard library should remain small and laser-focused on providing support for language primitives. The Foundation framework has the flexibility to include higher-level concepts and to build on top of the standard library, much in the same way that it builds upon the C standard library and Objective-C runtime on Darwin platforms.

##### Why include NSString, NSDictionary, NSArray, and NSSet? Aren't those already provided by the standard library?

There are several reasons why these types are useful in Swift as distinct types from the ones in the standard library:

* They provide reference semantics instead of value semantics, which is a useful tool to have in the toolbox.
* They can be subclassed to specialize behavior while maintaining the same interface for the client.
* They exist in archives, and we wish to maintain as much forward and backward compatibility with persistence formats as is possible.
* They are the backing for almost all Swift Array, Dictionary, and Set objects that you receive from frameworks implemented in Objective-C on Darwin platforms. This may be considered an implementation detail, but it leaks into client code in many ways. We want to provide them here so that your code will remain portable.

##### How do we decide if something belongs in the standard library or Foundation?

In general, the dividing line should be drawn in overlapping area of what people consider the language and what people consider to be a library feature.

For example, Optional is a type provided by the standard library. However, the compiler understands the concept to provide support for things like optional-chaining syntax. The compiler also has syntax for creating Arrays and Dictionaries.

On the other hand, the compiler has no built-in support for types like NSURL. NSURL also has ties into more complex functionality like basic networking support. Therefore this type is more appropriate for Foundation.

##### Why not make the existing Objective-C implementation of Foundation open source?

Foundation on Darwin is written primarily in Objective-C, and the Objective-C runtime is not part of the Swift open source project. CoreFoundation, however, is a portable C library and does not require the Objective-C runtime. It contains much of the behavior that is exposed via the Foundation API. Therefore, it is used on all platforms including Linux.

##### How do I contribute?

We welcome contributions to Foundation! Please see the [known issues](Docs/Issues.md) page if you are looking for an area where we need help. We are also standing by on the [mailing lists](https://swift.org/community/#communication) to answer questions about what is most important to do and what we will accept into the project.
