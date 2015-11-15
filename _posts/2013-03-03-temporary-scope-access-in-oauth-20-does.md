---
layout: post
title: Temporary scope access in OAuth 2.0 - Does it still exist?
date: '2013-03-03T12:55:00.000-05:00'
author: Shawn Drape
tags:
- security
- OAuth
- privacy
modified_time: '2013-03-03T12:55:18.665-05:00'
blogger_id: tag:blogger.com,1999:blog-4388783192727113653.post-5458053580856284128
blogger_orig_url: http://shawndrape.blogspot.com/2013/03/temporary-scope-access-in-oauth-20-does.html
---


So I was playing around with Asana this morning (still haven't gotten a great todo list system working so if you have recommendations there feel free to share) and noticed that they recently released an Android app. I figure that having a mobile app on my phone might make it more useful to me so I go and download it.

On first run I go to log in with my work account and it pops up an OAuth confirmation page. All good here so far: it asks for my e-mail address, basic profile information, and ability to manage my contacts. Wait, hold up. I don't like the idea of handing off my contact information (work has gotten a bit paranoid with contact info anyway since we had someone sign up with LinkedIn on a work e-mail and spam the entire contact list). So I hit no thanks, and the application hangs. Swell. (I'll point out that the application itself doesn't request access to the device's contact list).

So I decided to research why it wanted this contact list information. I can see it being useful once to populate a team contact list to get started but on an ongoing basis it seems like overkill. Long story short, Asana seems to be using the OpenID + OAuth hybrid method to handle authentication and resource authorization in a single call. While I can agree that this is better for the user experience I think demanding access to the contact list permanently during sign-up and login is a huge privacy concern.

Now, for those that don't know technically all OAuth access tokens are considered to be short lived. They have a limited time availability and after that they're done. When that happens, an application has the ability to request a new access token using a separate and long lived "refresh token". So I figure that's the solution there: Asana should only ask for refresh tokens for the main items they need for authentication and then just ask for the short lived access tokens whenever the user wants to do something like invite people to the workspace. The problem is, I don't actually see a way for the user to specify that anywhere. The reason for this is two-fold:

First, because all the requests are tied to a single refresh token, it must handle all the originally requested scopes. There is currently no mechanism which allows an application to separate out the scopes and tokens unless they make to explicit calls. This would prompt the user twice and makes for a poor experience.

Second, a user giving permission to various scopes don't get the ability to choose if they want that access to be short or long lived. They get an indication that the access may be long lived (I believe the keywords during the confirmation are "This application will be able to perform these actions when you are not logged in" or something similar) but it isn't the most clear notice and it's non-optional. A user's only choice is to either accept it, nor deny the access and not be able to use the application.

In the end, I decided to revoke Asana's rights on my account which I had been using on the desktop. I was only really experimenting with it and while it seems useful at this point my higher priority is on data security. Hopefully in the future, I won't have to make a choice between the two for Asana or any application.

If you agree with me that this sort of thing can be a problem, I'd love to hear from you in the comments.