# Prerequisites

- [Purescript Behaviors](https://github.com/paf31/purescript-behaviors-demo) => This library is used to get the reactive component features. (Effect, Signals)
  - Signals are the values that changes over time.
  - Effects are the SideEffect that runs a function in response to the signal changes.
  - We subscribe effects to the signals so the our UI can update on events

- [Purescript halogen vdom](https://github.com/purescript-halogen/purescript-halogen-vdom) => This library is used to create a VDOM, Props, Action Types.
  - If our state changes then we create a new VDOM Mapping and find the dif between the old & new VDOM
  - Then we update the actual DOM with the dif.
