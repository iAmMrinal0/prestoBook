## Using Presto commands to show the UI

So now that we have implemented the `UIRunner` let's start with getting to display our layout on the screen. To begin this we will start by writing a few types for the screen and it's respective actions in `src/Types.purs`.

```haskell
data MainScreen = MainScreen -- Screen name and type

data MainScreenAction = MainScreenAction -- Action name and type
```

Now that we have our basic screen type we will now create a few instances for these. To use Presto commands, our screen types need to fulfil a few type constraints. In this case our screen should have an `Encode` instance whereas our screen actions should have a `Decode` instance. Let's go ahead and add the respective instances:

```haskell
derive instance genericMainScreen :: Generic MainScreen _
instance encodeMainScreen :: Encode MainScreen where
  encode = genericEncode (defaultOptions { unwrapSingleConstructors = false })

derive instance genericMainScreenAction :: Generic MainScreenAction _
instance decodeMainScreenAction :: Decode MainScreenAction where
  decode = genericDecode (defaultOptions { unwrapSingleConstructors = true })
```

We have our instances ready but we still have an important aspect missing, which is, every screen and screen action requires an `Interact Error` instance too. We will now implement our instance for `MainScreen`

```haskell
instance interactMainScreen :: Interact Error MainScreen MainScreenAction where
  interact = defaultInteract
```

Now your `src/Types`, after the imports, should look like this:

```haskell
module Types where

import Control.Monad.Eff.Exception (Error)
import Data.Foreign.Class (class Decode, class Encode)
import Data.Foreign.Generic (defaultOptions, genericDecode, genericEncode)
import Data.Generic.Rep (class Generic)
import Presto.Core.Flow (class Interact, defaultInteract)

data MainScreen = MainScreen

data MainScreenAction = MainScreenAction

derive instance genericMainScreen :: Generic MainScreen _
instance encodeMainScreen :: Encode MainScreen where
  encode = genericEncode (defaultOptions {unwrapSingleConstructors = false})

derive instance genericMainScreenAction :: Generic MainScreenAction _
instance decodeMainScreenAction :: Decode MainScreenAction where
  decode = genericDecode (defaultOptions {unwrapSingleConstructors = true})

instance interactMainScreen :: Interact Error MainScreen MainScreenAction where
  interact = defaultInteract
```

And we are done with our types and instances. Let's get into showing the screen now. So to do that let's get back to `src/Main.purs`

Remember `#C` where we had a placeholder text for the app logic? That's were we are going to implement the functionality to show the screen. We will use a Presto method called `showUI` to show the screen.

```haskell
appFlow = showUI MainScreen
```

Time to run our app and see the results in action. The command to run is

```
$ pulp build --to index.js
```

Open `index.html` in your browser and you should see an input box and a submit button as our initial layout.

