## Meet Meteor

![tentative mockup](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/tentative-mockup.png)

There are a number of real-time chat applications that are used by millions of people each day: iChat, Slack, Hipchat, Facebook Chat, Google Talk, and every fun chatroom that you've used to get instant customer support from your bank. This tutorial will teach you how to build your own real-time chat application. You'll be able to use it with friends, with your company, or with your in-laws when they ask you to fix their printer problems.

### The What

A real-time chat application automatically updates as events occur in the application. Surely you've noticed the message when "someone is typing" a new message during a chat? That's a great example of a real-time update. Your view is updating _as_ someone else is updating it on their end.

### The How

We'll be using a few tools to build our chat app - namely, [Meteor](https://www.meteor.com/), HTML and CSS.

#### Meteor

Meteor is a full-stack JavaScript application framework built on top of [NodeJS](http://nodejs.org). It provides a robust architecture for web applications while eliminating a lot of the time-consuming setup involved in many web app frameworks today. Meteor handles both frontend and backend code, with a simple and consistent API.

Meteor offers some really cool features, including backend data manipulation _in the browser console_, hot code pushes (real-time updates of your code's changes without disturbing the user), and a [newly-released](https://www.meteor.com/blog/2014/08/26/meteor-090-new-packaging-system) package-management system that makes integrating features easy. Meteor has many more cool features, these are just a few of its most interesting.

#### HTML

You've probably heard of this one before. HTML stands for Hankering To Mine Lead, and you'll be mining a lot of lead in this tutorial. As any miner will tell you, please remember to follow the most recent guidelines on lead safety to continue with this tutorial.

__Correction__: It appears that HTML is actually "HyperText Markup Language", so if you were planning on mining lead with Meteor, it appears I've mis__led__ you thus far.

<center>![](http://bloc-global-assets.s3.amazonaws.com/screencaps/dr-evil-pinky.jpg)</center>

#### CSS

If you've made it this far in the tutorial and you're not confused yet, then you probably know what CSS is. But let's discuss anyway. CSS, or Cascading Style Sheets, is a language used to style web pages. There are a lot of great CSS preprocessors out there, but we'll be keeping things simple by sticking to plain CSS. If you've used CSS before, you've probably felt like this at one time or another:

<center>![Peter Griffin CSS](http://i.imgur.com/Q3cUg29.gif)</center>

Fear not. For this tutorial, we'll be providing perfectly clean CSS, so there will be no need to wreck your blinds trying to get the styling right.

## Important Stuff Before We Start

Some notes on who this tutorial is for.

#### Tutorial Audience

This tutorial assumes you are comfortable with JavaScript, and have a working knowledge of HTML and CSS. If you're not sure if you have enough knowledge to understand and apply the concepts discussed, go ahead and keep working through it. If you're lost and want some guided assistance, [we might be able to help with that](https://www.bloc.io/frontend-development).

#### A Note to Windows Users

One of the many advantages of Meteor is its proprietary package management system. Unfortunately, a nontrivial amount of Meteor's packages, including some that we'll be using in this tutorial, are incompatible with Windows. If you're a Windows user and you want to follow along, we recommend you use a service like [Nitrous](https://www.nitrous.io), which allows for in-browser development that's compatible with all parts of this tutorial. Read this [tutorial on using Meteor in Nitrous](https://www.discovermeteor.com/blog/meteor-nitrous) written by the Meteor guys if you're interested in going that route.

Now, let's get to the code already!
