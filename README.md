# Inline Expressions 

- In Swift, an inline expression is a code statement that is evaluated immediately when it is encountered in the program's execution flow. The result of the expression is used at the location where the expression is written. Using inline expressions can help to reduce code verbosity and improve code readability. Swift's @autoclosure attribute is one way to enable inline expressions. It enables you to define an argument that automatically gets wrapped in a closure, allowing expressions to be evaluated only when they are needed, rather than immediately. This is useful for performance optimization and code simplification.


```swift
func myFunc(_ closure: () -> Void) {
  closure()
}

var myVar = 0
myFunc { myVar = 1 }
print(myVar) // Output: 1
```

```swift
UIView.animate(withDuration: 1.0) {
  view.frame = CGRect(x: 0, y: 0, width: 100, height: 100)
}
```

```swift
let dictionary = ["name": "John"]
let age = (dictionary["age"] as? Int) ?? 25
print(age) // Output: 25
```

# @autoclosure

- The @autoclosure attribute in Swift allows you to define an argument that automatically gets wrapped in a closure, **deferring execution of potentially expensive expressions until they're actually needed.** This can make code more readable and expressive, as shown in examples such as inlining assignments, passing errors as expressions, and using type inference with default values. When used appropriately, @autoclosure can reduce verbosity and cruft in code while potentially improving performance.

```swift
func assert(_ expression: @autoclosure () -> Bool,
            _ message: @autoclosure () -> String) {
    guard isDebug else {
        return
    }

    // Inside assert we can refer to expression as a normal closure
    if !expression() {
        assertionFailure(message())
    }  
}

assert({ someCondition() }, { "Hey, it failed!" })

assert(someCondition(), "Hey it failed!")
```

```swift

UIView.animate(withDuration: 0.25) {
    view.frame.origin.y = 100
}

func animate(_ animation: @autoclosure @escaping () -> Void,
             duration: TimeInterval = 0.25) {
    UIView.animate(withDuration: duration, animations: animation)
}

animate(view.frame.origin.y = 100)

```

```swift
extension Optional {
    func unwrapOrThrow(_ errorExpression: @autoclosure () -> Error) throws -> Wrapped {
        guard let value = self else {
            throw errorExpression()
        }

        return value
    }
}

let name = try argument(at: 1).unwrapOrThrow(ArgumentError.missingName)
```

```swift
let coins = (dictionary["numberOfCoins"] as? Int) ?? 100

let coins = dictionary.value(forKey: "numberOfCoins", defaultValue: 100)

extension Dictionary where Value == Any {
    func value<T>(forKey key: Key, defaultValue: @autoclosure () -> T) -> T {
        guard let value = self[key] as? T else {
            return defaultValue()
        }

        return value
    }
}

```
