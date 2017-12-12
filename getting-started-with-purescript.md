# Getting Started with PureScript Presto

Before we get started with using Presto, we will begin with a simple HTML page with just an input box and a button.

```html
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

Name this `index.html` and load it in your browser and you should see an input box with a button. Let's now try to load this using JavaScript. Remove the contents from `body` section and add the snippet below after the `body` tag.

```html
<script type="text/javascript">
  function init() {
    document.body.innerHTML = `<input type="text" name="Input"/>
      <button>Submit</button>
    `
  }
  init()
</script>
```

Here we have a function `init` which is invoked right after we define it. So now that we have a simple page, let's get this running with Presto. The initialization of Presto requires implementations of 3 runners, and they are:

* UIRunner - how to run/show the UI
* APIRunner - how to interact or make API calls
* PermissionRunner - for mobile apps on how to get OS specific permissions



