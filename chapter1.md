# Basic Setup

Before we start with the code, let's install Presto with

```
bower install purescript-presto purescript-foreign-generic --save
```

Now, if you are asked for a suitable version for the `purescript-foreign-generic` package, select the option where version is `5.0.0` and above.

So now let's start with a basic html file

```html
<html>
  <head><title>ToDo App with Presto</title></head>
  <body>
    <script src="app.js"></script>
    <script src="index.js"></script>
  </body>
</html>
```

Here `app.js` is our DOM logic and `index.js` is our PureScript logic which we will get to in some time.

