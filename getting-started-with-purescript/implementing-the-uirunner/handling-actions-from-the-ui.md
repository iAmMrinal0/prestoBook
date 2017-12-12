## How to handle actions from the UI?

Similar to how we used the `showUI` function to show the screen, Presto provides a function called `runUI` which returns us an action. Remember we wrote types for action for a screen in `src/Types.purs`?

```haskell
data MainScreenAction = MainScreenAction
```

Let's now try to see if we can send a button click from the UI and handle that in PureScript. We will modify the init function and add a click handler to handle the button click

```js
function init () {
  document.body.innerHTML = `<input type="text" name="Input"/>
  <button onclick="buttonClick()">Submit</button>
  `
}

function buttonClick () {
  let event = {tag: "MainScreenAction"}
  window.callback(JSON.stringify(event))()
}
```

What we are doing here is that we match the action name we defined in the type and call the callback with it, notice the `()` at the end of the statement.

