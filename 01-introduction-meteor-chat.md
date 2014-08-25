## Introduction

![tentative mockup](https://dl.dropboxusercontent.com/u/10788831/Meteor%20Chat%20Assets/tentative-mockup.png)

Are you tired of web applications that are complicated and frustrating to build?

![building applications](https://38.media.tumblr.com/83e639ca53047e998ed50a98d2077ba7/tumblr_mltxo7v0aE1snk1roo1_400.gif)

That attract spam your mother definitely doesn't approve of?

![mom disapproves](https://38.media.tumblr.com/tumblr_mdf2i231dH1rje5o6o1_400.gif)

Then you've come to the right place! Introducing Bloc Chat*, the do-it-yourself real time chat application built in [Meteor](https://www.meteor.com).

_*Some assembly required._

## How??

Whoa! Hold your horses, double question mark. Let's take a second and introduce our stack for this crazy work of art.

#### Meteor

Meteor is a JavaScript application framework built on top of [NodeJS](http://nodejs.org) that provides a rapid prototyping architecture  for web applications. Meteor handles both the Frontend and Backend, with a simple, consistent API.

#### HTML

You've probably heard of this one before. HTML stands for Hankering To Mine Lead, and you'll be mining a lot of lead in this tutorial. As any miner will tell you, please remember to follow the most recent guidelines on lead safety to continue with this tutorial.

__Correction__: It appears that HTML is actually "HyperText Markup Language", so if you were planning on mining lead with Meteor, it appears I've mis__led__ you thus far (I promise that's maybe the last pun in this tutorial).

#### Stylus/CSS

- If stylus, make a joke about how Steve Jobs once said "if you see a stylus, they blew it"
- If CSS, include this gif ![Peter Griffin CSS](http://i.imgur.com/Q3cUg29.gif)

## Important Stuff Before We Start

Some notes on who this tutorial is for.

#### Tutorial Audience

This tutorial assumes you are comfortable building component-based JavaScript applications, and have a working knowledge of HTML and CSS. It also assumes you can read or have a Speak-and-Spell for your web browser. But seriously, we won't go super in-depth about those concepts, so it would help if you had some experience with them. If you're not sure if you have enough knowledge to understand and apply the concepts discussed, go ahead and keep working through it. If you're lost and want some guided assistance, [we might be able to help with that](https://www.bloc.io/frontend-development).

#### A Note to Windows Users

One of the many advantages of Meteor is its use of the [Meteorite Wrapper](http://oortcloud.github.com/meteorite/) to manage packages and installation of Meteor and some optional dependencies. Unfortunately, Meteorite [does not support Windows natively](http://oortcloud.github.io/meteorite/#notes), so if you want to follow along, we recommend you use a service like [Nitrous](https://www.nitrous.io), which allows for in-browser development that's compatible with all parts of this tutorial. Read this [tutorial on using Meteor in Nitrous](https://www.discovermeteor.com/blog/meteor-nitrous) written by the Meteor guys if you're interested in going that route.

## Giddy Up

It's time to put this baby in the oven and bake a chat-flavored cake.
