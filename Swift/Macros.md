## Freestanding

Macros that appear on their own, without being attached to a declaration.

```swift
func myFunction() {
	print("Currently running \(#function)")
	#warning("Something's wrong")
}
```

## Attached

Macros modify the declaration that they're attached to.
