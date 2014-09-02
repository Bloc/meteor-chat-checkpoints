## Introduction

![tentative mockup](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/tentative-mockup.png)

There are a number of real-time chat applications that are used by millions of people each day: Facebook chat, Google Talk/Google Hangouts, and every fun  chatroom that you've used to get instant customer support from your bank. This tutorial will help guide you through building your own real-time chat application, so if you ever start a bank, you can make people love talking to you, too!

### The What

By real-time chat, we mean a chat application that updates as events occur in the application. Seen the little message that lets you know when someone is typing in your chat window? That's an interface component that reacts to event that updates in real-time. Same with receiving messages very shortly after they're sent. No need to refresh the browser to get your messages; just sit there and wait for your fellow chat-ers to regale you with their thoughts _as they type and submit them_. If this still sounds interesting to you, keep reading.

### The How

We'll be using a few tools to make our chat app - namely, [Meteor](https://www.meteor.com/), HTML and CSS.

#### Meteor

Meteor is a JavaScript application framework built on top of [NodeJS](http://nodejs.org) that provides a robust architecture for web applications while eliminating a lot of the time-consuming setup involved in many web app frameworks today. Meteor handles both the Frontend and Backend, with a simple, consistent API.

Meteor offers some really cool features, including backend data manipulation _in the browser console_, hot code pushes (real-time updates of your code's changes without disturbing the user), and a [newly-released](https://www.meteor.com/blog/2014/08/26/meteor-090-new-packaging-system) package-management system that makes integrating features into Meteor quick and easy. Meteor has a lot more going for it -- these are just a few of its most interesting features.

#### HTML

You've probably heard of this one before. HTML stands for Hankering To Mine Lead, and you'll be mining a lot of lead in this tutorial. As any miner will tell you, please remember to follow the most recent guidelines on lead safety to continue with this tutorial.

__Correction__: It appears that HTML is actually "HyperText Markup Language", so if you were planning on mining lead with Meteor, it appears I've mis__led__ you thus far (I promise that's maybe the last pun in this tutorial).

#### CSS

If you've made it this far in the tutorial and you're not confused yet, then you probably know what CSS is (but we'll talk about it anyway). CSS, or Cascading Style Sheets, is a language used to style web pages. There are a lot of great CSS preprocessors out there, but we'll be keeping things simple by sticking to plain, ol' CSS. If you've used CSS before, you've probably felt like this at one time or another:
![Peter Griffin CSS](http://i.imgur.com/Q3cUg29.gif)

But for this tutorial, we'll be providing the CSS, so no need to wreck your blinds trying to get the styling right.

## Important Stuff Before We Start

Some notes on who this tutorial is for.

#### Tutorial Audience

This tutorial assumes you are comfortable building component-based JavaScript applications, and have a working knowledge of HTML and CSS. It also assumes you can read or have a Speak-and-Spell for your web browser. But seriously, we won't go super in-depth about those concepts, so it would help if you had some experience with them. If you're not sure if you have enough knowledge to understand and apply the concepts discussed, go ahead and keep working through it. If you're lost and want some guided assistance, [we might be able to help with that](https://www.bloc.io/frontend-development).

#### A Note to Windows Users

One of the many advantages of Meteor is its proprietary package management system. Unfortunately, a nontrivial amount of Meteor's packages, including some that we'll be using in this tutorial, are incompatible with Windows.  If you're a Windows user and you want to follow along, we recommend you use a service like [Nitrous](https://www.nitrous.io), which allows for in-browser development that's compatible with all parts of this tutorial. Read this [tutorial on using Meteor in Nitrous](https://www.discovermeteor.com/blog/meteor-nitrous) written by the Meteor guys if you're interested in going that route.

## Giddy Up

It's time to put this baby in the oven and bake a chat-flavored cake.
