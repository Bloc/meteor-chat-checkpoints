## Creating User Accounts

![user login](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/05_user_login_start_gif.gif)

It was fun running Meteor `.insert()` calls in the browser console, but it's time to make our app more secure. Appropriately, the Meteor package that enables us to do inserts in the browser - or in our client-side code - is called `insecure`. Let's remove it.

```bash(Terminal)
$ meteor remove insecure
```

With this package removed, we don't have to worry about nefarious users creating data without our consent.

Speaking of users...

#### Packages for User Accounts

Meteor has two packages that take care of basic login functions:
- [`accounts-password`](http://docs.meteor.com/#accounts_passwords) encrypts passwords on submit, and
- [`accounts-base`](http://docs.meteor.com/#accounts_api) handles standard account operations like logging in and detecting a `current user` in the application.

`accounts-base` also gives us the general ability to add user documents to our database, which is important for listing our chat participants later. We'll add both to our application.

```bash(Terminal)
$ meteor add accounts-base
$ meteor add accounts-password
```

### Hiding Data and Templates Behind Log In

Let's hide some of our views that should only be accessible to logged in users. The `accounts-base` package has a `{{currentUser}}` template helper that we'll use in an `{{#if}}` block to hide our `messageList` and `newMessage` templates.

>We don't want non-logged-in users looking at our messages, or sending them.

Open `layout.html` and add the conditional block.

```HTML(client/views/application/layout.html)
 <template name="layout">
   <aside class="sidebar">
     <h5 class="room-label">Participants <i class="ion-person-stalker"></i></h5>
   <section class="main-container">
     <header class="main-header">JabberBlocy</header>
-    {{> messageList}}
-    {{> newMessage}}
+      {{#if currentUser}}
+        {{> messageList}}
+        {{> newMessage}}
+      {{else}}
+        {{> loginPage}}
+      {{/if}}
   </section>
 </template>
```

The `{{else}}` renders a `loginPage` partial when a user isn't logged in. Let's create that template and add some styling so its design is simple, but consistent with the rest of our application.

#### The Login Page Template

Let's create the file to hold our template.

```bash(Terminal)
$ touch client/views/application/login_page.html
```

Add the markup to the new file.

```html(client/views/application/login_page.html)
<template name="loginPage">
  <div class="login-overlay">
    <div class="login-container">
      <h4 class="login-title">Chat Log In</h4>
      <form class="login-form">
        <label for="username">Username</label>
        <input type="text" name="username" placeholder="username">
        <label for="password">Password</label>
        <input type="password" name="password" placeholder="&bull;&bull;&bull;&bull;&bull;&bull;">
        <input type="checkbox" name="newuser">
        <label class="checkbox-label">New User?</label>
        <button type="submit" class="login-button">
          Log in
        </button>
      </form>
    </div>
  </div>
</template>
```

Once again, we'll be using the `name` attributes on the `input` tags to select our form values on submit. We've also added some unicode bullets (`&bull;`) for the placeholder in our password input so that we can see those recognizable dots before a user types anything. Because this is a chat tutorial and not a user-onboarding tutorial, we'll keep things simple and add a checkbox at the bottom of the inputs that, when checked, will _create_ a user account _instead of logging one in_.

And we can't forget our styling.

```css(client/stylesheets/styles.css)
/* all other styles are above this */

// make our login a modal on top of an overlay
.login-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1000;
  background-color: rgba(60,60,60,0.3);
}

// center the login box on the modal
.login-overlay .login-container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate3d(-50%,-50%,0);
  -webkit-transform: translate3d(-50%,-50%,0);
  -moz-transform: translate3d(-50%,-50%,0);
  width: 280px;
  height: 350px;
  padding: 36px;
  background-color: white;
}

.login-container .login-title {
  font-family: "Open Sans";
  font-size: 36px;
  text-align: center;
  margin-bottom: 30px;
}

.login-form label {
  font-family: "Open Sans";
  margin-bottom: 8px;
  display: block;
  width: 100%;
}

.login-form input[type="text"],
.login-form input[type="password"] {
  font-family: "Open Sans";
  box-sizing: border-box;
  width: 100%;
  padding: 12px;
  outline: none;
  border: 1px solid rgb(89,90,92);
  margin-bottom: 8px;
}

.login-form input[type="text"]:focus,
.login-form input[type="password"]:focus {
  border: 1px solid rgb(18,114,184);
}

.login-form .login-button {
  width: 100%;
  margin-top: 8px;
  padding: 16px;
  background-color: rgb(71,57,129);
  border-radius: 6px;
  border: 0;
  font-family: "Open Sans";
  font-size: 16px;
  color: white;
  cursor: pointer;
}

.login-container .checkbox-label {
  font-family: "Open Sans";
  font-size: 14px;
  line-height: 16px;
  vertical-align: middle;
  color: rgb(78,146,196);
  cursor: pointer;
  display: inline;
}
```

If all went according to plan you should see something similar to this when you load your app locally:

![chat login styling](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/05_chat_login_styling.png)

#### Adding Authentication Logic to the Login Page

We'll need to add some code to a new file called `login_page.js` where we'll put the logic that will authenticate our users in a `Template.loginPage.events()` call. It might look similar to the submit event we defined for `newMessage`.

```bash(Terminal)
$ touch client/views/application/login_page.js
```

```js(client/views/application/login_page.js)
Template.loginPage.events({
  'submit .login-form': function(e) {
    e.preventDefault();

    // store username, password, and the new user checkbox in variables
    var un = $(e.target).find('[name="username"]');
    var pw = $(e.target).find('[name="password"]');
    var nu = $(e.target).find('[name="newuser"]');

    // define a function that tells us whether there's input
    var fieldsPopulated = function() {
      return un.val() !== '' && pw.val() !== '';
    };

    // if the new user checkbox isn't selected
    if (fieldsPopulated() && !nu.is(':checked')) {
      Meteor.loginWithPassword(un, pw, function(err) {
          // clear our inputs
          un.val('');
          un.val('');
        }
      });
    // and if it is...
    } else if (fieldsPopulated() && nu.is(':checked')) {
      Accounts.createUser({
        username: un.val(),
        password: pw.val()
      });
    } else {
      alert("There was an error logging you in");
    }

  }
});
```

We've added some very minor error handling that tells our authentication how to differentiate between a new user and a returning user login. `Meteor.loginWithPassword` is a method provided for login by the `accounts-base` package. `Accounts.createUser` came from `accounts-password`, and creates a user with a securely hashed password using the popular [`bcrypt`](http://en.wikipedia.org/wiki/Bcrypt) algorithm.

`Accounts.createUser` also updates the `Meteor.user()` method upon creation, which is Meteor's internal function for tracking the current user. We'll be using this to attribute data to certain users later.

### Listing Users in the Participants Bar

Now that we've got a way to create users, let's add a way to show a list of the users in the sidebar. Following a similar convention to the message views, we'll create two templates and a JavaScript file for user list. To stay consistent with our UI, we'll be using the term _participants_ for our components.

Let's create a separate folder to hold our participant files, and then add the files to it.

```bash(Terminal)
$ mkdir client/views/participants
$ touch client/views/participants/participant_list.html
$ touch client/views/participants/partipants_list.js
$ touch client/views/participants/participant_item.html
```

We'll keep our participant list really simple for now.

```HTML(client/views/participants/participant_list.html)
<template name="participantList">
  {{#each participants}}
    {{> participantItem}}
  {{/each}}
</template>
```

And our `participantItem` template will be similarly sparse.

```HTML(client/views/participants/participant_item.html)
<template name="participantItem">
  <div class="user-item">
    <i class="ion-chatbubble"></i>
    {{username}}
  </div>
</template>
```

To populate our templates with the proper data, we'll need to add a query to our users using a method from `accounts-base` defined in another `Template.helpers()` block.

```js(client/views/participants/participant_list.js)
Template.participantList.helpers({
  participants: function() {
    return Meteor.users.find().fetch();
  }
});
```

To continue our clean aesthetic, we'll add small chat bubbles adjacent to our users' names and some styling for the classes that we added to the markup in `participantItem`.

```css(client/stylesheets/styles.css)
/ add this to the bottom of the stylesheet */

.user-item {
  font-family: "Open Sans";
  color: white;
  font-weight: 300;
  margin: 8px 0;
}

.user-item .ion-chatbubble {
  font-size: 12px;
  line-height: 16px;
  margin-right: 4px;
  vertical-align: middle;
}
```

Meteor's hot code reloads should populate the sidebar for you without having to refresh the page.

![finished screenshot](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/05_completed_participant_sidebar.png)
