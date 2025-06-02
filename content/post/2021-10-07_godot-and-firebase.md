---
layout: post
title: "Godot and Firebase - Part 2"
subtitle: "Login with Username/Password"
date: 2021-10-07
author: "mars3142"
URL: "/2021/10/07/godot-and-firebase/"
image: "/img/2021/06/09/godot-and-firebase/godot_source_code.webp"
categories: [godot]
tags:
  - tutorial
  - firebase
---

![Godot source code](/img/2021/06/09/godot-and-firebase/godot_source_code.webp)

The [first part](/2021/06/09/godot-and-firebase/) of this tutorial was about the backend setup and this part will handle how to use the Godot Engine to combine the custom backend and Firebase to create a login with username and password.

I highly recommend a layered architecture (like [Game Development Center](https://www.youtube.com/c/GameDevelopmentCenter)'s playlist for [Dedicated Multiplayer](https://www.youtube.com/playlist?list=PLZ-54sd-DMAKU8Neo5KsVmq8KtoDkfi4s)), but within my sample I use only a single Godot project with the minimum code to use Firebase with Godot.

Because we want to test our login within Firebase, we need the latest version of the GodotFirebase plugin. You can find it [here](https://github.com/GodotNuts/GodotFirebase). This plugin will be used to read data from Firebase with your username/password account.

{{< gist mars3142 b8f8c092ae4c067b34402db167cf3d68 >}}

As you can see, this is the simple code for create/login to the backend server and using the token to connect to Firebase. The important parts will be described now.

In my scene are two TextEdit fields and a couple of buttons. The TextEdit fields are referenced in line 6 and 7. These are the username and password fields, so we can input some data. Line 12 and 13 are the custom signals from the plugin. They are needed to do a login, get user data (from Firebase Auth). Line 15–32 are the button pressed callbacks and you can see, three of them are calling our custom backend with just a slightly different parameter list.

The important code are from line 35 (function \_request_custom_backend) and from line 46 (function \_on_http_request_request_completed).

We add a special header (x-action) into every request, so we know in the response, which request was fired, because our backend is sending this header back to the client.

Now that we have a backend server for own game, we can create a server within eg Google Cloud with Kubernetes. But this could be another post with docker images, deployment files and an example project for review.
