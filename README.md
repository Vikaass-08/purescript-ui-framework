# Prerequisites

- [Purescript Behaviors](https://github.com/paf31/purescript-behaviors-demo) => This library is used to get the reactive component features. (Effect, Signals)
  - Signals are the values that changes over time.
  - Effects are the SideEffect that runs a function in response to the signal changes.
  - We subscribe effects to the signals so the our UI can update on events

- [Purescript halogen vdom](https://github.com/purescript-halogen/purescript-halogen-vdom) => This library is used to create a VDOM, Props, Action Types.
  - If our state changes then we create a new VDOM Mapping and find the dif between the old & new VDOM
  - Then we update the actual DOM with the dif.

```purescript

-- Get window.document() instance
foreign import getDocument :: Effect Document

-- Basically append child inside the document.body()
foreign import attachMachine :: forall a b. Step a b -> Effect Unit

createScreen :: forall state action. Screen state action -> Effect (Effect Unit)
createScreen screen = do

  -- Create an empty event reciever as a base to recieve events for the behavior
  -- push is a function that will call all subscribers to the event
  ({push, event} :: EventIO action) <- create

  -- Make shift mechanism to draw UI
  doc <- getDocument

  -- VDOM
  machine <- runEffectFn1 (buildVDom (spec doc)) $ screen.view push screen.state
  attachMachine machine

  -- Create a reference
  latestMachine <- new machine

  -- Connect the behavior to the event and use state as the base
  let stateBehavior = unfold screen.update event screen.state

  -- Sample the behavior whenever the event triggers
  let stateEvent = sample_ stateBehavior event

  -- Trigger UI update whenever state changes
  stateEvent `subscribe` (\state -> updateUI latestMachine screen push state)

```
