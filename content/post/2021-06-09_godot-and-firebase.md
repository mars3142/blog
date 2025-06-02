---
layout: post
title: "Godot and Firebase"
subtitle: "Login with Username/Password"
date: 2021-06-09
author: "mars3142"
URL: "/2021/06/09/godot-and-firebase/"
image: "/img/2021/06/09/godot-and-firebase/godot_source_code.webp"
categories: [godot]
tags:
  - tutorial
  - firebase
---

![Godot source code](/img/2021/06/09/godot-and-firebase/godot_source_code.webp)

Multiplayer games need some form of user authentication. To build a custom backend is a hard part, but to create secure user authentication is another story. For saving the user/game state you can use different approaches. In my case I wanted to use Firebase, but the primary authentication for Firebase is via email and password. This will be a two part tutorial for using Godot with Firebase with username and password.

I found many bad solutions for that, which I would never recommend for that purpose. Most of time I read "add a fake domain, so you create an email address". But the worst advice was, read credentials from Realtime Database with the json options within the game client. This is only possible, if you allow everybody to read the complete user collection. **DON'T ALLOW THAT.**

For the communication between Godot and Firebase I found a useful module: https://github.com/GodotNuts/GodotFirebase. This module is able to login into Firebase and use Cloud Firestore as a persistence layer. But also this module used email and password for authentication. So I created a discussion to use an official feature of Firebase: Login with custom token.

The module can't create a custom token, because don't trust the client. So you need to generate in a trusted environment: your server. Because we want to use Firebase, I go the full route and created an API with Firebase Functions, which my backend game server can connect. So my API will never be exposed to the client and because I use also a custom header within my request, no one with knowledge of my server URI can use the API.

Because the code is large and I just want to give an overview how to do it, I will just paste some snippets so you understand the process. If needed, I could create a sample project.

To start with the authentication, we need to store our users with username and password. I used the idea from above with the Realtime Database, but my database is unreadable for everybody (only admin accounts like Firebase Functions can access the data). To ensure that the user credentials are save (not recoverable or visible), I use the node package firebase-scrypt. This will use some parameters, which can be found within the auth section of Firebase behind the three dots (or you use your own).

{{< gist mars3142 53dac924f07bddf45e565fd8fa993a72 >}}

The important part starts at line 25 and following. Every user get's a random salt and the password is created from the data given by the user (which also can be hashed once on the client as well). In line 32 we store everything in our user node within the Realtime Database.

![Firebase Realtime Database](/img/2021/06/09/godot-and-firebase/firebase_user.webp)

{{< gist mars3142 d77df1db6cbbbb9ef2107f4041b90f65 >}}

The signin part is not that difficult. It’s the line 27. You need the password, the stored salt and hash to verify if everything is correct. That’s why we stored everything in our database.

{{< gist mars3142 baf148df2ee0ed3d3146341deb84660f >}}

This is the most important code, because in line 2 we create the custom token for logging into Firebase. This token is a normal JWT and can be used to login into Firebase.

![Firebase Auth](/img/2021/06/09/godot-and-firebase/firebase_auth.webp)

In the next part I’ll show you how to connect the API and Firebase with that custom token.
