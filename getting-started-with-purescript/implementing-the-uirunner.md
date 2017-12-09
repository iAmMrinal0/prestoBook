# The UIRunner

For our current task, we will be implementing the `UIRunner` and get into the other runners later as we want to use them. Create a directory `src` then a file inside called `Runner.js` in it where we will define our runners.

```
exports["showUI'"] = function(sc) {
  return function (screen) {
    return function() {
      var screenJSON = JSON.parse(screen);
      window.showScreen(sc, screenJSON);
    }
  }
};
```



