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

```js
window.showScreen = function (callback, data) {
    window.callback = callback;
    handleScreenAction(data);
};

function handleScreenAction (data) {
  init()
}
```

Basically the flow is that, when the app is run, it triggers an `event` and calls the callback with the required data for the screen which then calls `handleScreenAction` with the data passed. So we will now call `init()` inside `handleScreenAction` instead of invoking automatically, so don't forget to remove the `init()` invoke statement at the end.

Let's start with the PureScript code to show our initial layout. The following code goes in `src/Main.purs`

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
  let runtime = Runtime uiRunner permissionRunner apiRunner
      freeFlow = evalStateT (run runtime appFlow)
  launchAff (makeVar' empty >>= freeFlow)
  where
    uiRunner :: UIRunner
    uiRunner screen = pure "UIRunner" -- UIRunner to be implemented here #A

    apiRunner :: APIRunner
    apiRunner req = pure "APIRunner" -- APIRunner to be implemented here #B

    permissionRunner :: PermissionRunner
    permissionRunner = PermissionRunner permissionCheckRunner permissionTakeRunner

    permissionCheckRunner :: PermissionCheckRunner
    permissionCheckRunner perms = pure PermissionGranted

    permissionTakeRunner :: PermissionTakeRunner
    permissionTakeRunner perms =  pure $ map (\x -> Tuple x PermissionDeclined) perms

main = launchFreeFlow
```

We implemented a `showUI'` function before which we will now use in the `#A` section. But before we do that we need to import it. PureScript has the concept of `Foreign Function Interface` which allows us to interact with code written in JavaScript \(or whichever backend you are using.\) And so for our `showUI'` function, the import definition looks something like this:

```haskell
foreign import showUI' :: forall e. (String -> Eff (ui :: UI | e) Unit) ->  String -> (Eff (ui :: UI | e) Unit)
```

Add the necessary imports required for `Eff` and `UI` which are defined in:

```haskell
import Control.Monad.Eff (Eff)
import Presto.Core.Types.App (UI)
```

Now that we have our type definition and import, let's use the `showUI'` for the UIRunner in `#A`.

```haskell
uiRunner :: UIRunner
uiRunner screen = makeAff (\err success -> showUI' success screen)
```

Essentially what we are doing here is that, we have a function called `makeAff` which takes a function as an argument. The function takes two arguments: an error and a success callback. So we call `showUI'` with the success callback and the screen while disregarding the error callback. Now our `src/Main.purs` should look like this:

```haskell
module Main where

import Prelude

import Control.Monad.Aff (launchAff, makeAff)
import Control.Monad.Aff.AVar (makeVar')
import Control.Monad.Eff (Eff)
import Control.Monad.State.Trans (evalStateT)
import Data.StrMap (empty)
import Data.Tuple (Tuple(..))
import Presto.Core.Flow (APIRunner, PermissionCheckRunner, PermissionRunner(PermissionRunner), PermissionTakeRunner)
import Presto.Core.Language.Runtime.Interpreter (Runtime(..), UIRunner, run)
import Presto.Core.Types.App (UI)
import Presto.Core.Types.Permission (PermissionStatus(..))

foreign import showUI' :: forall e. (String -> Eff (ui :: UI | e) Unit) ->  String -> (Eff (ui :: UI | e) Unit)

appFlow = pure unit -- App logic to be implemented here #C

launchFreeFlow = do
  let runtime = Runtime uiRunner permissionRunner apiRunner
      freeFlow = evalStateT (run runtime appFlow)
  launchAff (makeVar' empty >>= freeFlow)
  where
    uiRunner :: UIRunner
    uiRunner screen = makeAff (\err success -> showUI' success screen)

    apiRunner :: APIRunner
    apiRunner req = pure "APIRunner" -- APIRunner to be implemented here #B

    permissionRunner :: PermissionRunner
    permissionRunner = PermissionRunner permissionCheckRunner permissionTakeRunner

    permissionCheckRunner :: PermissionCheckRunner
    permissionCheckRunner perms = pure PermissionGranted

    permissionTakeRunner :: PermissionTakeRunner
    permissionTakeRunner perms =  pure $ map (\x -> Tuple x PermissionDeclined) perms

main = launchFreeFlow
```

Next we will see how to show our initial layout using Presto commands.

