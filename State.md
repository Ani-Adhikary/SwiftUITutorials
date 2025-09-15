
<h2>What is @State? (short answer) </h2>

@State is a SwiftUI property wrapper that makes a value a local source of truth for a view. When a @State value changes, SwiftUI automatically re-runs the view’s body so the UI stays in sync.

Use @State for small, local, view-owned pieces of state (booleans, counters, text fields, toggles). Don’t use it for shared or long-lived model objects — for those use @StateObject, @ObservedObject, or @EnvironmentObject.
