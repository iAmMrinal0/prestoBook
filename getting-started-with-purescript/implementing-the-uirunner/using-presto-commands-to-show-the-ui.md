## Using Presto commands to show the UI

So now that we have implemented the `UIRunner` let's start with getting to display our layout on the screen. To begin this we will start by writing a few types for the screen and it's respective actions.

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

We have our instances ready but we still have an important aspect missing, which is, every screen and screen action require an Interact instance too.



Add the necessary imports for Generic , genericEncode, genericDecode, defaultOptions

