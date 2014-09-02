## Creating Messages

Our app still looks nice, and we have some sample data that we've provided to the templates, but it would be nice if we had a real data model where we could store new messages about cats and our banking problems.

### The Message Collection

This is where Meteor [collections](http://docs.meteor.com/#collections) come in. Collections act differently depending on their context. On the server side, collections act as an interface to our MongoDB backend, making changes to our database in response to calls on the frontend. On the client side, a collection is a (generally) a subset of the data from our DB that is updated in real-time.

Let's get a better idea of what this means by creating our first collection. We'll be putting it in the `collections` directory that we created earlier.

```bash(Terminal)
$ touch collections/messages.js
```

> Code that is defined outside of the `server` and 'client' directories is accessible to both.

We'll open up our new `messages.js` file and add a single line to create our first collection.

```js(collections/message.js)
Messages = new Meteor.Collection('messages');
```
> You might be wondering why we're not preceding this collection definition with `var`. Using `var` restricts the scope of the variable to the current file, and since we want to perform operations on this collection on both the client and server side, we want to make sure `Messages` is a global variable accessible from everywhere.

#### Connecting Messages to Our `messageList` Template

Let's replace the old static data that we had in `message_list.js` with a query to our Messages collection.

```js(client/views/messages/message_list.js)
-messageData = [
-  {
-    name:    "Joe Lipper",
-    content: "Wow building a chat app with Meteor is so easy!"
-  },
-  {
-    name:    "Mike Jewett",
-    content: "Yeah wow! What a great framework. I can't wait to keep building this thing out!"
-  },
-  {
-    name:    "Joe Lipper",
-    content: "Hang in there -- we've only scratched the surface on this thing."
-  }
-]
-
Template.messageList.helpers({
- messages: messageData
+ messages: function() {
+   return Messages.find();
+ }
});
```

Getting rid of the `messageData` means that we now have an empty message list. __But__ we also have a persistent data source that we can use populate it. Let's move the array into a file that will pre-populate our chat with messages everytime a new database is created or reset.

We'll start by creating a file called `fixtures.js` that will hold our data that we'll pre-populate.

```bash(Terminal)
$ touch server/fixtures.js
```

And we'll add our `messageData` array along with a conditional that will populate the database with a few messages if there aren't any present.

```js(server/fixtures.js)
var messageData = [
  {
    name: "Joe Lipper",
    content:  "Wow building a chat app with Meteor is so easy!"
  },
  {
    name: "Mike Jewett",
    content:  "Yeah wow! What a great framework. I can't wait to keep building this thing out!"
  },
  {
    name: "Joe Lipper",
    content:  "Hang in there -- we've only scratched the surface on this thing."
  }
];

if (Messages.find().count() === 0) {
  for (var i = 0; i < messageData.length; i++) {
    Messages.insert({
      name: messageData[i].name,
      content:  messageData[i].content
    });
  }
}
```

Meteor's hot code reloads should populate your template immediately, but if it doesn't for whatever reason, stop the server (using `ctrl + c`) and reset the DB. Start the server again after the reset.

```bash(Terminal)
$ meteor reset # reset the db
$ meteor # start the server again
```

If you're familiar with MongoDB, you might recognize the `.find()`, `.count()` and `.insert()` methods used in `fixtures.js`. These are all available to Meteor thanks to a watered-down implementation of MongoDB, affectionately referred to as Minimongo.

>You can see which Mongo methods are accessible with Minimongo by reading through the [`Meteor.Collection` documentation](http://docs.meteor.com/#find). We'll cover everything you'll need for this tutorial, though.

#### Experimenting with Minimongo

One of the coolest parts of Minimongo is that with the proper packages installed, Meteor makes it accessible from the browser console. That's right -- you can manipulate the DB right from your browser.

You can test it out by navigating to [localhost](http://localhost:3000) and typing in some of the commands we used in `fixtures.js`. If you open up a second browser window, you can see that the data syncs instantly.

![browser sync](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/meteor_collection_sync.gif)

Your first reaction might be: that's cool, but isn't it _very_ insecure?

And you'd be right -- if this were the way that production Meteor apps were configured. We'll add security checks and get rid of the in-browser inserts in future chapters. For now, enjoy how easy it is to test data manipulation from the console :).

### An Interface for Creating New Messages

With our Messages collection hooked up to our DB, we can add an interface for creating new messages and wire it up with our backend to create messages on submit.

Let's create a `new_message.html` file and add some styling to it so it'll look nice in our app.

```bash(Terminal)
$ touch client/views/messages/new_message.html
```

Inside we'll paste this markup:
```html(client/views/messages/new_message.html)
<template name="newMessage">
  <form class="new-message">
    <textarea name="content" class="new-message-input" placeholder="Start typing a new message..."></textarea>
    <button type="submit" class="new-message-submit">Send</button>
  </form>
</template>
```

And add our styling for it by updating `styles.css`:
```css(client/stylesheets/styles.css)
/* ... */
.message-list {
  position: absolute;
  top: 70px;
  width: 100%;
+ bottom: 126px;
  overflow-y: scroll;
  padding-right: 36px;
  padding-left: 36px;
  box-sizing: border-box;
}

/* ... */
.message-item .message-content {
  font-family: "Open Sans";
  color: rgb(89,90,92);
  font-size: 14px;
  margin-bottom: 16px;
}
+
+.new-message {
+  width: 100%;
+  box-sizing: border-box;
+  position: absolute;
+  bottom: 0;
+  padding: 32px;
+  display: table;
+  background-color: white;
+}
+
+.new-message .new-message-input, .new-message .new-message-submit {
+  display: table-cell;
+  vertical-align: middle;
+}
+
+.new-message .new-message-input {
+  border: 1px solid transparent;
+  width: 85%;
+  padding: 16px;
+  height: 28px;
+  background-color: rgb(242,243,247);
+  font-family: "Open Sans";
+  font-size: 14px;
+  transition: all 0.25s ease-in;
+  -webkit-transition: all 0.25s ease-in;
+}
+
+.new-message .new-message-input:focus {
+  outline: none;
+  border: 1px solid rgb(18,114,184);
+}
+
+.new-message .new-message-submit {
+  border: 0;
+  padding: 16px 24px;
+  margin-left: 16px;
+  color: white;
+  height: 60px;
+  border-radius: 6px;
+  background-color: rgb(18,114,184);
+  font-weight: 700;
+  text-transform: uppercase;
+}
+
+/* Make our placeholder styling the same as our input text */
+::-webkit-input-placeholder {
+   font-family: "Open Sans";
+   font-size: 14px;
+}
+
+:-moz-placeholder { /* Firefox 18- */
+   font-family: "Open Sans";
+   font-size: 14px;  
+}
+
+::-moz-placeholder {  /* Firefox 19+ */
+   font-family: "Open Sans";
+   font-size: 14px;  
+}
+
+:-ms-input-placeholder {  
+   font-family: "Open Sans";
+   font-size: 14px;  
+}
```

And finally, let's update `layout.html` with a reference to our partial:
```html(client/views/application/layout.html)
<template name="layout">
  <aside class="sidebar">
    <h5 class="room-label">Participants <i class="ion-person-stalker"></i></h5>
  </aside>
  <section class="main-container">
    <header class="main-header">JabberBlocy</header>
    {{> messageList}}
+   {{> newMessage}}
  </section>
</template>
```

The result should be something like the screenshot below.
![chat with submit](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/fininshed_screenshot_03_with_submit.png)

#### Wiring Up Our Template to Submit Data

To submit the data server side, Spacebars has a method on the `Template` object called `.events()` that allows us to execute some code based on an event and a selector for that event. We'll create a `new_message.js` file and add the `.events()` call to it.

```bash(Terminal)
$ touch client/views/messages/new_message.js
```

And in the file:
```js(client/views/messages/new_message.js)
Template.newMessage.events({
  'submit .new-message': function(e) {
    e.preventDefault();

    var contentText = $(e.target).find('[name=content]');

    var msg = {
      name:    "Meteor Chatter",
      content: contentText.val()
    };

    msg._id = Messages.insert(msg);
    contentText.val('');
  }
});
```

We used a little jQuery to select the content of the textarea and get its value. With that, and a fake name, we can make an `.insert()`` call and create our messages on the submit event. If you've copied everything correctly, you should be able to post your favorite inanities in the form and see them update the `messages` data immediately!

![working meteor submit](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/new_message_send.gif)
