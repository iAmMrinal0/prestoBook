# Getting Started with PureScript Presto

Before we get started with using Presto, we will begin with a simple HTML page with just an input box and a button.

```
<html>
  <head>
    <title>ToDo App with Presto</title>
  </head>
  <body>
    <input type="text" name="Input"/>
    <button>Submit</button>
  </body>
</html>
```

Name this `index.html` and load it in your browser and you should see an input box with a button. Let's now try to load this using JavaScript. Add the snippet below after the `body` tag.

    <script type="text/javascript">
      function init() {
        document.body.innerHTML = `<input type="text" name="Input"/>
          <button>Submit</button>
        `
      }
      init()
    </script>

Here we have a function `init` which we invoke right after we define it. So now that we have a simple page, let's get this running with Presto. The initialization of Presto requires 3 implementations of runtimes, and they are:

* UIRunner - how to run/show the UI
* APIRunner - how to call API's
* PermissionRunner - for mobile apps on how to get OS specific permissions

For our current task, we will be implementing the UIRunner and go deep into the other runners later as we want to use them. Create a file called `src/Main.purs` in the present directory.

