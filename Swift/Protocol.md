## Extension & Default Implementation

Swift's `protocol` can be extended using `extension` like `class`, `struct` and `enum`, in which we can add functions and computed properties to it.

Implement the requirements of the `protocol` to provide default implementations, so that any `class`, `struct` and `enum` that confirms to this protocol can use them.

```swift
protocol MyProtocol {
    func foo()
}

extension MyProtocol {
    func foo() {
        print("Default implementation of foo")
    }
}

struct MyProtocolImpl: MyProtocol {}
```
