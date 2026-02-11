# iOS (Swift) Code Review Checklist

Use this checklist when reviewing `.swift` files. Focus on ARC memory management, UI thread safety, and Swift idioms.

## 1. Memory Management (ARC)
- **Retain Cycles**: Check all closures (blocks) capturing `self`. Is `[weak self]` or `[unowned self]` missing?
- **Delegate Pattern**: Are delegate protocols defined as `AnyObject` (class-bound) and properties declared `weak`?
- **Task Cancellation**: Are async `Task` handles stored and cancelled in `deinit` or `viewDidDisappear` when appropriate?

## 2. Threading & Concurrency
- **UI on Main**: Are UI updates (updating labels, reloading tables) explicitly dispatched to `MainActor` or `DispatchQueue.main`?
- **Async/Await**: Is strict concurrency checking respected? Are actors used to protect mutable state?
- **Data Races**: Look for mutable arrays/dictionaries accessed from multiple dispatch queues without locks or actor isolation.

## 3. Swift Best Practices
- **Force Unwrapping**: Flag all usage of `!` (force unwrap). Suggest `if let`, `guard let`, or nil-coalescing `??`.
- **Access Control**: Are properties `private` or `private(set)` by default? Are classes `final` unless inheritance is intended?
- **Value Types**: Are `struct` used over `class` for data models to ensure thread safety and immutability?
- **String Literals**: Flag hardcoded strings in code. Suggest using `Localized` strings or generated constants (R.swift/SwiftGen).

## 4. UIKit / SwiftUI Specifics
- **Reuse Identifiers**: In UIKit, are cells properly reused?
- **View Body (SwiftUI)**: Is the `body` property free of side effects and heavy computation?
- **State Management**: Are `@State`, `@Binding`, and `@ObservedObject` used correctly to prevent unnecessary redrawing?

## 5. Security (iOS Specific)
- **Keychain**: Is sensitive data (tokens, passwords) stored in Keychain, not `UserDefaults`?
- **App Transport Security**: Are arbitrary HTTP loads disabled?