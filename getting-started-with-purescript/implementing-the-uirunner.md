# The UIRunner

For our current task, we will be implementing the `UIRunner` and get into the other runners later as we want to use them. Create a directory `src` and a file inside called `Main.js` where we will define our runners.

```JavaScrip
exports["showUI'"] = function(successCallback) {
  return function (screen) {
    return function () {
      var screenJSON = JSON.parse(screen);
      window.showScreen(successCallback, screenJSON);
    }
  }
};
```

The above function is a curried function which takes a `successCallback` and a `screen` which internally calls a `showScreen` function which we define in `index.html` before our `init` function:

```
window.showScreen = function (callback, data) {
    window.callback = callback;
    handleScreenAction(data);
};

function handleScreenAction (data) {
}
```

Basically the flow is that, when the app is run, it triggers an `event` and calls the callback with the required data for the screen. Now we will start with the PureScript code to show our initial layout. The following code goes in `src/Main.purs`

```haskell
module Main where

import Prelude

import Control.Monad.Aff (launchAff)
import Control.Monad.Aff.AVar (makeVar')
import Control.Monad.State.Trans (evalStateT)
import Data.StrMap (empty)
import Data.Tuple (Tuple(..))
import Presto.Core.Flow (APIRunner, PermissionCheckRunner, PermissionRunner(PermissionRunner), PermissionTakeRunner)
import Presto.Core.Language.Runtime.Interpreter (Runtime(..), UIRunner, run)
import Presto.Core.Types.Permission (PermissionStatus(..))

appFlow = pure unit -- App logic to be implemented here #C

launchFreeFlow = do
  let runtime = Runtime uiRunner (PermissionRunner permissionCheckRunner permissionTakeRunner) apiRunner
      freeFlow = evalStateT (run runtime appFlow)
  launchAff (makeVar' empty >>= freeFlow)
  where
    uiRunner :: UIRunner
    uiRunner a = pure "UIRunner" -- UIRunner to be implemented here #A

    apiRunner :: APIRunner
    apiRunner req = pure "APIRunner" -- APIRunner to be implemented here #B

    permissionCheckRunner :: PermissionCheckRunner
    permissionCheckRunner perms = pure PermissionGranted

    permissionTakeRunner :: PermissionTakeRunner
    permissionTakeRunner perms =  pure $ map (\x -> Tuple x PermissionDeclined) perms

main = launchFreeFlow
```

We implemented a `showUI'` function before which we will now use in the `#A` section



