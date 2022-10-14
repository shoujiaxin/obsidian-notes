## All property wrappers

### `@AppStorage`

Reads and writes values from `UserDefaults`. This owns its data.

```swift
struct ContentView: View {
    @AppStorage("username") var username: String = "Anonymous"

    var body: some View {
        VStack {
            Text("Welcome, \(username)!")

            Button("Log in") {
                username = "@twostraws"
            }
        }
    }
}
```

### `@Binding`

Refers to value type data owned by a different view. Changing the binding locally changes the remote data too. This does not own its data.

```swift
struct ContentView: View {
    @State private var showingAddUser = false

    var body: some View {
        VStack {
            // your code here
        }
        .sheet(isPresented: $showingAddUser) {
            // show the add user view
        }
    }
}
```

### `@Environment`

Reads data from the system, such as color scheme, accessibility options, and trait collections. You can also add custom keys. This does not own its data.

```swift
@Environment(\.colorScheme) private var colorScheme
@Environment(\.horizontalSizeClass) var horizontalSizeClass
@Environment(\.managedObjectContext) var managedObjectContext
```

### `@EnvironmentObject`

Reads a shared object that we inject into the environment. Can only use one `@EnvironmentObject` wrapper per `ObservableObject`  type per view. This does not own its data.

```swift
// pass to view
let myView = MyView().environmentObject(theViewModel)

// inside the view
@EnvironmentObject var viewModel: ViewModelClass
```

### `@FetchRequest`

Starts a Core Data fetch request for a particular entity. This owns its data.

```swift
@FetchRequest(
    sortDescriptors: [
        SortDescriptor(\.name, order: .reverse)
    ],
    predicate: NSPredicate(format: "surname == %@", "Hudson")
) var users: FetchedResults<User>
```

### `@FocusedBinding`

Designed to watch for values in the key window, such as a text field that is currently selected. This does not own its data.

### `@FocusedValue`

A simpler version of `@FocusedBinding` that doesn’t unwrap the bound value for you. This does not own its data.

### `@FocusState`

### `@GestureState`

Stores values associated with a gesture that is currently in progress, such as how far you have swiped, except it will be reset to its default value when the gesture stops. This owns its data.

```swift
struct ContentView: View {
    @GestureState var dragOffset: CGSize = .zero

    var body: some View {
        Color.accentColor
            .frame(width: 50, height: 50)
            .padding()
            .offset(dragOffset)
            .gesture(
                DragGesture()
                    .updating($dragOffset) { value, state, _ in
                        state = value.translation
                    }
            )
    }
}
```

### `@Namespace`

Creates an animation namespace to allow matched geometry effects, which can be shared by other views. This owns its data.

### `@NSApplicationDelegateAdaptor` / `@UIApplicationDelegateAdaptor`

Creates and registers a class as the app delegate for a macOS / iOS app. This owns its data.

### `@ObservedObject`

Refers to an instance of external class that conforms to the `ObservableObject` protocol. The object could be created multiple times as the view is created.

### `@Published`

Creates observable objects that automatically announce when changes occur. SwiftUI will automatically monitor for such changes, and re-invoke the `body` property of any views that rely on the data. This owns its data.

### `@ScaledMetric`

Reads the user's Dynamic Type setting and scales numbers up or down based on an original value you provide. This owns its data.

```swift
struct ContentView: View {
    @ScaledMetric(relativeTo: .largeTitle) var imageSize = 100.0

    var body: some View {
        Image(systemName: "cloud.sun.bolt.fill")
            .resizable()
            .frame(width: imageSize, height: imageSize)
    }
}
```

### `@SceneStorage`

Saves and restores small amounts of data for state restoration. This works a bit like [`@AppStorage`](#AppStorage), but rather than working with `UserDefaults`, it instead gets used for state restoration. This owns its data.

```swift
struct ContentView: View {
    @SceneStorage("text") var text = ""

    var body: some View {
        NavigationView {
            TextEditor(text: $text)
        }
        .navigationViewStyle(.stack)
    }
}
```

### `@State`

Manipulates small amounts of value type data locally to a view. This allow us to modify values inside a struct. This owns its data.

### `@StateObject`

Used to store new instances of reference type data that conforms to the `ObservableObject` protocol. The object will only be created once. This owns its data.

## Storing temporary data

- [`@State`](#State)
- [`@Binding`](#Binding)
- [`@StateObject`](#StateObject)
- [`@ObservedObject`](#ObservedObject)
- [`@EnvironmentObject`](#EnvironmentObject)
- [`@Published`](#Published)

## Storing long-term data

- [`@AppStorage`](#AppStorage)
- [`@SceneStorage`](#SceneStorage)
- [`@FetchRequest`](#FetchRequest)

## Reading environment data

- [`@Environment`](#Environment)
- [`@ScaledMetric`](#ScaledMetric)

## Referring to views

- [`@Namespace`](#Namespace)

## Application handling

Get access to the old `NSApplicationDelegate` or `UIApplicationDelegate`:

- [`@NSApplicationDelegateAdaptor` / `@UIApplicationDelegateAdaptor`](#NSApplicationDelegateAdaptor%20UIApplicationDelegateAdaptor)

## Source of truth

Property wrappers that create and manage values directly:

- [`@AppStorage`](#AppStorage)
- [`@FetchRequest`](#FetchRequest)
- [`@GestureState`](#GestureState)
- [`@Namespace`](#Namespace)
- [`@NSApplicationDelegateAdaptor` / `@UIApplicationDelegateAdaptor`](#NSApplicationDelegateAdaptor%20UIApplicationDelegateAdaptor)
- [`@Published`](#Published)
- [`@ScaledMetric`](#ScaledMetric)
- [`@SceneStorage`](#SceneStorage)
- [`@State`](#State)
- [`@StateObject`](#StateObject)
