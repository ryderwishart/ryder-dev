---
title: Django Drive-Through Analogy
date: "2020-02-29"
description: "Django architecture explained in a simple and relatable analogy."
---

Looking for a short description of Django architecture and an analogy so you don't forget this time?

Look no further than the drive through.

## How does Django fit into a user experience?

The way a web app works is like this:

![A Django web app architecture](/basic-django-structure.png)

*[Source](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Introduction)*

1. Users get on their device and connect to your app somehow.

2. A user sends some signals to your app.

3. The app interprets those signals (via some kind of URL-mapping module, such as `urls.py`).

4. The app then "handles" that signal via a request-handler module, like `views.py`. *The tutorial notes that it makes sense to handle each resource via a different view function. The URL mapper connects the right requests with the right view functions.*

5. The app looks, through `views.py`, into its database (`models.py`), and maybe changes something.

6. The app then sends some signals back to the user's device using `views.py`, sometimes referencing some kind of layout template, e.g. `blog-layout.html`. *Maybe this is where React.js will fit in?*

## The Django view component

From what I can see, the view component of a Django app takes in and spits out HTML.

It gets HTML requests from the user, and then gives the user an HTML response.

The kind of response it gives depends on how it interacts with the model component, which is the database of the app.

## The Django model component

The model defines what kind of data the app uses. It's also the place where the data is stored.

So how does all of this work together?

## Django by drive-through analogy

Django's architecture can be understood by analogy to a drive-through order.

A **user** pulls up to the drive through. And little speaker takes their requests (their *input*).

This speaker is like the **URL-mapping** component. It sends the requests inside the building so the customer is not just yelling out of their car window as a sign.

The signals go inside to whichever headset-wearing worker is designated for handling orders. This worker is the **view** component.

The view component takes down the user's order. But this order only makes sense if it includes requests the restaurant can fulfill. How does the worker know what orders can be fulfilled? By looking at the menu, of course! The menu is like the **model** component.

The menu defines what kinds of orders can be made. Do you want fries with that?

The worker finds out not only whether certain orders fit the menu, but also whether the raw materials are actually there. In other words, the model component doesn't just tell you what kind of orders the restaurant fulfills, but it also allows you to query existing instances of that data.

When the user pulls up to the window, the view component (who took the order) returns information about payment, and, let's hope, actually hands over the meal. The meal is like the **response**. 

But if you've ever been to a drive through, then you know they they don't just pour the fries into your lap, maybe splashing a soft drink on top ([coning](https://www.youtube.com/watch?v=78DBf2_9Esg), anyone?).

No. Each restaurant has its own conventions for handing stuff over. Drinks come in a cup. More than one cup comes in a tray. Food is in a big, and the straws and napkins are in there too.

This convention for packaging the response is like the **template**. The template gives the view a scaffolding it can fill up with response items.

Simple, right?
