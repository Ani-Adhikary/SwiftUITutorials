
<h2>What is @State? (short answer) </h2>

@State is a SwiftUI property wrapper that makes a value a local source of truth for a view. When a @State value changes, SwiftUI automatically re-runs the view’s body so the UI stays in sync.

Use @State for small, local, view-owned pieces of state (booleans, counters, text fields, toggles). Don’t use it for shared or long-lived model objects — for those use @StateObject, @ObservedObject, or @EnvironmentObject.

<h2>Quick rules</h2>

Declare: @State private var foo = initialValue.

Mutate the variable directly (e.g., foo += 1) — SwiftUI will update the view.

To give child views read/write access, pass a binding with $foo.

Keep @State private to the view (encapsulation).

Update on the main thread (UI updates).

<h2>Example: Counter — step-wise (copy-paste runnable)</h2>

```swift
import SwiftUI

@main
struct MyCounterApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}

struct ContentView: View {
    // 1) Declare local state owned by this view:
    @State private var count: Int = 0

    var body: some View {
        VStack(spacing: 20) {
            // 2) Read the state to show UI:
            Text("Count: \(count)")
                .font(.largeTitle)

            HStack(spacing: 16) {
                // 3) Mutate the state — UI updates automatically:
                Button("−") { count -= 1 }
                    .font(.title)
                    .padding()
                    .buttonStyle(.bordered)

                Button("+") { count += 1 }
                    .font(.title)
                    .padding()
                    .buttonStyle(.bordered)
            }

            // 4) Pass a binding down to a child view:
            ChildIncrementView(count: $count)
                .padding(.top)
        }
        .padding()
    }
}

struct ChildIncrementView: View {
    // 5) Receive a binding to the parent's state:
    @Binding var count: Int

    var body: some View {
        Button("Increment from child") {
            count += 1
        }
        .buttonStyle(.borderedProminent)
    }
}
    
```

<h2>Step-wise explanation of the example </h2>

@State private var count: Int = 0 — tells SwiftUI: “store count for this view and watch it for changes.”

When you read count in body, the UI reflects the current value.

When you change count (e.g., count += 1), SwiftUI marks the view as needing update and re-evaluates body.

$count produces a Binding<Int> — a two-way connector. Passing $count to ChildIncrementView allows the child to change the parent’s state.

@Binding var count: Int in ChildIncrementView receives that two-way binding and mutates it directly.

<h2>Common @State use-cases</h2>

Toggle switches: @State private var isOn = false → Toggle("On?", isOn: $isOn)

Text input: @State private var name = "" → TextField("Name", text: $name)

Showing/hiding sheets or alerts: @State private var showSheet = false

<h2>When not to use @State</h2>

For shared or complex data models — use ObservableObject + @StateObject or @ObservedObject.

For app-wide environment state — use @EnvironmentObject.

For persistence across launches — use AppStorage or save to disk/DB.

<h2>Gotchas & tips</h2>

Mark it private (encapsulation).

@State survives view body recomputations, but if the view is removed and recreated (different identity), the state resets. Use id(_:) carefully if you want a new identity.

Update state on the main thread. Changing @State from a background thread can produce warnings/errors.

Use @State for simple value types; for classes with many properties prefer ObservableObject and @StateObject.
