## Templating with Meteor

![finished product](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/finished_screenshot_03_without_submit.png)

Let's start dividing our markup into templates. Meteor has its own templating system called [Spacebars](https://github.com/meteor/meteor/blob/devel/packages/spacebars/README.md) (inspired by [Handlebars](http://handlebarsjs.com/)) that we'll be using to insert our different blocks of HTML.

### Restructuring Our Views

Let's start by creating a new `views` directory called `application` where we'll be putting our templates that will persist throughout all components of the chat app.

```bash(Terminal)
$ mkdir client/views/application
```

Inside, we'll create a `layout.html` file where we'll hold all of our the content from our `body` tag.

```bash(Terminal)
$ touch client/views/application/layout.html
```

#### Using Partials to Insert Our Layout Template

We'll be using our first Spacebars feature here, called __partials__. Partials allow us to put HTML blocks (usually in separate files) within other templates. In `main.html`, let's take out the content we had in between the `body` tags and put it into `layout.html`.

```HTML(layout.html)
<template name="layout">
  <aside class="sidebar">
    <h5 class="room-label">Participants <i class="ion-person-stalker"></i></h5>
  </aside>
  <section class="main-container">
    <header class="main-header">JabberBlocy</header>
  </section>
</template>
```

Notice that we've wrapped our HTML in a special `<template>` tag. The `name` attribute of this tag allows us to identify the template in other HTML files. Using this name, we can now replace that markup in `main.html` and use the Spacebars syntax for partials.

```HTML(main.html)
<head>
  <title>JabberBlocy</title>
  <link rel="stylesheet" type="text/css" href="http://yui.yahooapis.com/3.17.2/build/cssreset/cssreset-min.css">
  <link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Open+Sans:300,400,700">
  <link rel="stylesheet" type="text/css" href="http://code.ionicframework.com/ionicons/1.5.2/css/ionicons.min.css">
</head>
<body>
+ {{> layout}}
</body>
```

Spacebars uses double curly braces to signify that some kind of Spacebars logic will be populated in this part of the template. In this case, the greater than (`>`) symbol is Spacebar's way of denoting that we want to insert a partial here. Notice that the name following the `>` is identical to our `name` attribute on the `<template>` tag. This is how Spacebars knows which template to insert where.

### Creating More Templates

Now that we know how partials work, we can start creating some more templates to be included as partials in our existing templates. The first we'll create is a template called `messageList` that will hold the markup for the messages in our chatroom.

Let's create a `messages` directory in our views where we'll hold all of our message-related partials and put our first template file in it, called `message_list.html`.

```bash(Terminal)
$ mkdir client/views/messages
$ touch client/views/messages/message_list.html
```

Even though we haven't populated it yet, let's add the reference to the `messageList` template in `layout.html`.

```html(client/views/application/layout.html)
<template name="layout">
  <aside class="sidebar">
    <h5 class="room-label">Participants <i class="ion-person-stalker"></i></h5>
  </aside>
  <section class="main-container">
    <header class="main-header">JabberBlocy</header>
+   {{> messageList}}
  </section>
</template>
```

We'll fill the `messageList` template with our necessary markup, and include a nice feature of Spacebars called a __block helper__, along with a reference to another partial within that block.

```html(client/views/messages/message_list.html)
<template name="messageList">
  <div class="message-list">
    {{#each messages}}
      {{> messageItem}}
    {{/each}}
  </div>
</template>
```

The `{{#each}}` block helper in this template will allow us to insert a partial called `messageItem` for every element in the array of data called `messages`. Where does that `messages` data come from, and how can we associate with the message list template? For that, we have Spacebar's [template helpers](http://docs.meteor.com/#template_helpers).

#### Template Managers and Helpers

Meteor likes to keep its JavaScript logic separate from its templates. Template helper definitions allow us to maintain that separation. The convention in Meteor is to name the JavaScript file eponymously with the template that it is managing. Let's create the file that will hold our helper logic and manage our data for us.

```bash(Terminal)
$ touch client/views/messages/message_list.js
```

Inside `message_list.js`, we'll create an array of messages that we'll pass to the `{{#each}}` block helper from earlier.

```js(client/views/messages/message_list.js)
messageData = [
  {
    name:    "Joe Lipper",
    content: "Wow building a chat app with Meteor is so easy!"
  },
  {
    name:    "Mike Jewett",
    content: "Yeah wow! What a great framework. I can't wait to keep building this thing out!"
  },
  {
    name:    "Joe Lipper",
    content: "Hang in there -- we've only scratched the surface on this thing."
  }
];

Template.messageList.helpers({
  messages: messageData
});
```

>Notice that when we call the `helpers` method on the `Template`, we use the middle part to identify the precise name of the template we want the helper to work with.

If you've dealt with an [MVVM framework](http://en.wikipedia.org/wiki/Model_View_ViewModel) like Angular before, you might be thinking that this behavior looks consistent with that of a controller. Template helpers are similar, but generally only act as a place to hold data that the templates can act on. Other logic that might be held in a controller is defined elsewhere in Meteor (we'll get into that in a bit).

Now that we have data that we can use to populate our individual messages with, we can create our `messageItem` partial that we referenced in the `{{#each}}` block helper in `message_list.html`. We'll call the file that holds the template `message_item.html` and put it in our `messages` directory with the other message views.

```bash(Terminal)
$ touch client/views/messages/message_item.html
```

And we'll add our markup

```HTML(client/views/messages/message_item.html)
<template name="messageItem">
  <div class="message-item">
    <h6 class="message-sender">{{name}}</h6>
    <p class="message-content">
      {{content}}
    </p>
  </div>
</template>
```

Notice that the properties of the objects in our `messageData` array are accessible in the `messageItem` template. This is because the template is included in the scope of the `{{#each}}` block, so every individual template is passed the object that corresponds to its instance. Meteor treats the template like a wrapper object for the data, which makes it accessible simply by property name within the template (no need to prepend `messageItem` to them).

### Styling

To bring the look our templates up-to-date with the app, we'll be adding some styling to `style.css`.

```css(client/stylesheets/styles.css)
/* ... */
.main-container .main-header {
  background-color: white;
  padding: 24px;
  font-family: "Open Sans";
  color: rgb(157,158,164);
}
+
+.message-list {
+  position: absolute;
+  top: 70px;
+  width: 100%;
+  overflow-y: scroll;
+  padding-right: 36px;
+  padding-left: 36px;
+  box-sizing: border-box;
+}
+
+.message-item {
+  position: relative;
+  width: 100%;
+  border-bottom: 1px solid rgb(227,229,232);
+}
+
+.message-item .message-sender {
+  font-family: "Open Sans";
+  color: rgb(50,51,54);
+  font-size: 14px;
+  font-weight: 700;
+  margin-top: 16px;
+  margin-bottom: 12px;
+}
+
+.message-item .message-content {
+  font-family: "Open Sans";
+  color: rgb(89,90,92);
+  font-size: 14px;
+  margin-bottom: 16px;
+}
```

If you aren't running your app already, do so by executing `meteor` in the project's root directory and make sure you see something like the screenshot at the top.
