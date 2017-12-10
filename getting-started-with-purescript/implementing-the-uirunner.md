# The UIRunner

For our current task, we will be implementing the `UIRunner` and get into the other runners later as we want to use them. Create a directory `src` and a file inside called `Main.js` where we will define our runners.

```JavaScrip
exports["showUI'"] = function(successCallback) {
  return function (screen) {
    return function() {
      var screenJSON = JSON.parse(screen);
      window.showScreen(successCallback, screenJSON);
    }
  }
};
```

The above function is a curried function which takes a `successCallback` and a `screen` which internally calls a `showScreen` function which we define in `index.html` before our `init` function:

```
window.showScreen = function (callBack, data) {
    window.callBack = callBack;
    handleScreenAction(data);
};
```



