## Up and Running

### Installation

To install Meteor, open your preferred command line interface. Mac users will probably use [Terminal](http://en.wikipedia.org/wiki/Terminal_(OS_X). Windows users should open a new application in Nitrous.

To install Meteor for Mac users, you'll need to run:

```bash(Terminal)
$ curl https://install.meteor.com | sh
```

### Creating and Configuring Our App

Once Meteor has successfully installed, you'll be able to use the Meteor command line tool to create your app.

```bash(Terminal)
$ meteor create jabberblocy
```

You can choose your own name for your chat app, but at Bloc, we believe in terrible play-on-words. In that spirit we've named ours JabberBlocy. We'll also be referring to the app by that name for the rest of the tutorial, so if you've chosen a different name, keep that in mind. Check to make sure the app was created properly by running `ls <app name>` in your current directory. You should see three files, all with an eponymous name to your app.

```bash
jabberblocy.css
jabberblocy.html
jabberblocy.js
```

Let's delete the files now so we can re-structure our app using some of Meteor's conventions for app configuration.

```bash(Terminal)
$ rm -rf jabberblocy.{js,html,css}
```

#### The Standard App Directory Structure

Meteor has strong opinions about how to structure your app. Let's create the five standard Meteor app directories.

```bash(Terminal)
$ mkdir -p lib/ server/ client/ collections/ public/
```

What files you choose to put in these directories affects where they're processed (server or client side) and when they're loaded by the app. If you're into guessing, you might have pre-supposed that code in `client` is meant for the client only, and `server` is meant for the server only. And you'd be right.

Code in `lib` is loaded before any other code in the app. Static assets go in the `public` directory. We'll go deeper into what `collections` are later.

### Creating Our First Views
Let's create our first views in the `client` directory. We'll create a `views` subdirectory, and add two files: `main.html` and `main.js`.

```bash(Terminal)
$ mkdir client/views
$ touch client/views/main.html
$ touch client/views/main.js
```

> Note that Meteor loads any files with `main.*`, last.

Meteor follows a convention of placing JavaScript and HTML files related to a particular view component (i.e. a part of the app distinguished by its use or feature) in the same place.

Use your favorite text editor to open up `main.html`, and let's add our first markup:

```html(client/views/main.html)
<head>
  <title>JabberBlocy</title>
  <link rel="stylesheet" type="text/css" href="http://yui.yahooapis.com/3.17.2/build/cssreset/cssreset-min.css">
  <link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Open+Sans:300,400,700">
  <link rel="stylesheet" type="text/css" href="http://code.ionicframework.com/ionicons/1.5.2/css/ionicons.min.css">
</head>
<body>
  <aside class="sidebar">
    <h5 class="room-label">Participants <i class="ion-person-stalker"></i></h5>
  </aside>
  <section class="main-container">
    <header class="main-header">JabberBlocy</header>
  </section>
</body>
```

A couple of things to note: we're using the ever-useful (and [recently retired](http://yahooeng.tumblr.com/post/96098168666/important-announcement-regarding-yui)) [YUI Reset Styles](http://yuilibrary.com/yui/docs/cssreset/) to clear any pre-packaged browser styling. We'll also be using the open-source [ionicons](http://ionicons.com/) icon library to add a little visual flavor to our app. Finally, we'll be including the [Open Sans Typeface](https://www.google.com/fonts/specimen/Open+Sans) to give us a slick sans-serif font.

For styling, we'll want to create a separate `stylesheets` directory in `client`, and within that directory, we'll create `styles.css`.

```bash(Terminal)
$ mkdir client/stylesheets
$ touch client/stylesheets/styles.css
```

Next, let's add our first style properties to our new `styles.css`.

```css(client/stylesheets/styles.css)
.sidebar {
  width: 20%;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  background: linear-gradient(rgb(71,57,129), rgb(18,114,184));
  padding: 24px 16px;
}

.sidebar .room-label {
  font-family: "Open Sans";
  color: white;
  font-weight: 300;
  letter-spacing: 1px;
}

.main-container {
  width: 80%;
  top: 0;
  right: 0;
  bottom: 0;
  position: absolute;
  box-sizing: border-box;
  background-color: rgb(242,243,247);
}

.main-container .main-header {
  background-color: white;
  padding: 24px;
  font-family: "Open Sans";
  color: rgb(157,158,164);
}
```

Now that we have something to look at, let's run our app:

```bash(Terminal)
$ meteor # make sure you're in the root directory of the app
```

You should see some output like:
```
13:57:36 joelipper@Joes-MacBook-Pro.local jabberblocy meteor
[[[[[ ~/Development/meteor/jabberblocy ]]]]]

=> Started proxy.
=> Started MongoDB.
=> Started your app.

=> App running at: http://localhost:3000/
```

Navigate to [localhost:3000](http://localhost:3000/) and you should see the beginning  of our app.

![empty home page](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/beginning_home_page.png)

To stop the Meteor server, just press control-C in your Terminal.

Daaaaaayuummmm (is probably what you're thinking). Hold that "a" a little longer - we've got some more goodies coming up.
