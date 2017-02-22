
# Auth-Boss
Become an Auth Boss. Learn about different Auth methodologies.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents** 

- [Introduction](#introduction)
  - [Who](#who)
  - [Why](#why)
  - [How](#how)
- [Examples cases](#examples-cases)
- [General Best Practices](#general-best-practices)
- [Terminology](#terminology)
  - [Terms](#terms)
    - [Encryption](#encryption)
    - [HTTP](#http)
    - [HTTPS](#https)
    - [TLS / SSL](#tls--ssl)
    - [Cookie](#cookie)
    - [Sessions](#sessions)
    - [2 (3, 4) Factor authentication](#2-3-4-factor-authentication)
- [Methodologies](#methodologies)
  - [Technologies](#technologies)
- [Resources and Footnotes](#resources-and-footnotes)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
# Introduction

The intention of this document is to chronicle and catalogue the methodologies of _authentication_ on the web. By _authentication_ I am referring to, in the simplest sense, the process of creating a system through which user's can "login" to an online service and be given access to otherwise protected resources.

This quote might better sum up what I'm trying to cover:

> Client authentication involves proving the identity of a client (or user) to a server on the Web. We will use the term “authentication” to refer to this problem.[1]

***

## Who
I am a self-taught developer with a passion for open source tech, learning, mentoring, and knowledge-sharing. 

## Why
I am writing this guide, because I felt woefully at a loss for finding resources that explained safe, smart and current technologies and methods for authentication in a simple and concise manner. I decided it was time to put my research hat on and do some leg work. 

## How
My writing style aims to be simple, terse, and to leverage layperson terms over jargon and tech-words. 

**Disclaimer** : This document does not serve as an all-encompassing catalogue of all authentications methods for the web; nor does this document purport to provide the "best" methods of authentication.

**Regarding mistakes, errors, and inconsistencies**: I am bound to make mistakes and miss many things. I AM NOT A SECURITY EXPERT. If you see something that could be improved make a pull request explaining what I missed + what I can improve. Bonus points if you're not a jerk about it.

# Examples cases  

I will use a single example across this document to illustrate a login flow both in terms of what is happening on the "client" (the user in front of their computer) and what is occurring on the "server" (behind the scenes, so to speak).

Our example cases will feature a new imaginary friend: `Beorn`. Beorn likes knitting, and often goes to `http://www.knittingworld.com` to buy quilting supplies. Beorn has a user account on `knittingworld` and we will see him continuously login across the examples in this document. 

# General Best Practices

Before jumping into the technologies that exist for managing authentication, let's consider some best practices / things you should never do.

Some of these items following may not directly pertain to login/authentication/user signups, but are generally useful and good to know. 

- Never store passwords as plain text in a database.
- Never write your own hashing algorithm (unless you're really really smart)
- Do not write your own authentication technology, again unless you're really really smart. 
- Use [HTTPS](https://en.wikipedia.org/wiki/HTTPS). 

**Some quotes**

> We also found that many Web sites would design their own authentication mechanism to provide a better user experience. Unfortunately, designers and implementers often do not have a background in security and, as a result, do not have a good understanding of the tools at their disposal[2]

> the best defense against such attacks is to use SSL with some form of client authentication[2]

# Terminology

There is a quite a bit of jargon in this sub-set of web development. At the time of learning about these things, I found myself increasingly frustrated with the seemingly limitless number of jargon / shop talk, tech terms that I didn't understand ( and my respective increasing laziness finally encouraged me to write this doc and learn what the hell this junk is). Below is a list of terms that you will see again and again when it comes to authentication. Each word will link to a defintion I have found, but the description written will be my attempts to create a concise summary / defintion in lay-person terms.

## Terms 

### [Encryption](https://en.wikipedia.org/wiki/Encryption)

From Wiki:
> Encryption is the process of encoding messages or information in such a way that only authorized parties can access it.

### [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)

Stands for Hyper Text Transfer Protocol. This is a big one, and I'm not sure I can do it justice to explain simply without being reductive. The web is built around HTTP - it's the technology used to communicate between web servers and users. 

Your browser is considered an HTTP client - because it sends _requests_ to an HTTP server. There are many different kind of _requests_ that your client can make — you may have heard of some of the most popular ones — `GET` `POST` `PUT` and `DELETE`.

HTTP servers send _responses_ to your browser — the client — these responses come in the form of _Resources_. Resources could be (but are not limited to): HTML files, images, text, JSON, and more. You can (kind of) think of `resources` as "files" that come back from a server.

_Other Links on this topic_:

- [HTTP Made Really Easy](http://www.jmarshall.com/easy/http/#whatis)
- [RFC2616 ](https://tools.ietf.org/html/rfc2616) — this is a massive document-spec-thing on HTTP. It is listed as outdated, but also lists the documents that have superseded it.
- [rfc7235](https://tools.ietf.org/html/rfc7235) — http documentation specifically regarding Authentication. Another large, and (in my opinion) scary looking doc. I have not read this, nor the link above.


### [HTTPS](https://en.wikipedia.org/wiki/HTTPS)

HTTPS is HTTP with more security. It goes hand in hand with `SSL / TLS` Originally made popular for payment transaction over the internet, it has become much more popular lately. You may recognize https as `that green text that shows up to the left of my url in the browser`; often accompanied by an icon of a lock, or something of the sort.

HTTPS is built on top of HTTP but adds a layer of encryption using SSL/TLS to protect traffic between browsers and servers.

HTTPS encrypts the information that's sent along with your HTTP requests. This is particular important when we start talking about authentication!

From wikipedia:

> HTTPS creates a secure channel over an insecure network. This ensures reasonable protection from eavesdroppers and man-in-the-middle attacks, provided that adequate cipher suites are used and that the server certificate is verified and trusted.

### [TLS / SSL](https://en.wikipedia.org/wiki/Transport_Layer_Security)

TLS and SSL are a cryptographic protocol. TLS and SSL encrypt the data you send across a network — it is designed to prevent people from "eavesdropping" or tampering with the data that you are sending.

There are some useful videos on youtube that help to explain some of these complex topics, but unfortunately I can't speak to the veracity of them all. This [video by MIT opencourseware](https://www.youtube.com/watch?v=S2iBR2ZlZf0) looks relatively useful!

### [Cookies](https://en.wikipedia.org/wiki/HTTP_cookie)

Cookies, at the very base level, are small pieces of data that get stored on a _user's browser_. Cookies, in contrast to HTTP are _stateful_ — meaning that HTTP cannot store user information, but cookies can. 

A common example of a web cookie: 

You visit `http://knittingworld.com` to buy some nice patterns and materials for your next knitting project. You log in, and add three items to your cart. As you are about to "check out" in the online store and ship the items to your home, you hear a loud "bang!" and realize that you left a can of tuna in the microwave on high. Not good! You close the browser, instantly forgetting about your new quilt goodies,  and  go clean up the mess. After the smell of canned tuna has finally sank into the walls and carpet of your home, you return to your computer and revisit `https://knittingworld.com` ... only to find... your items are still in your cart. How?! Cookies.

There are different kinds of cookies. Some cookies will stick around in your browser for many days, while others will disappear as soon as you close your browser. 

Cookies have played a big role in authentication in the past. Authentication cookies are commonly used by web servers to know that user is logged in or not; allowing a server to know whether or not the client can have access to otherwise protected routes. 

Sometimes cookies are considered nerfarious or insecure (see [cross site scripting](https://en.wikipedia.org/wiki/Cross-site_scripting) or [cross-site requiest forgery](https://en.wikipedia.org/wiki/Cross-site_request_forgery)). This can be the case, but there are also methods that can be taken to increase the security of cookies. For example, you can set a `secure` flag on a cookie meaning it can only be transmitted over an encrypted connection (HTTPS`).

[Persistent cookies](https://en.wikipedia.org/wiki/HTTP_cookie#Persistent_cookie) sometimes carry a negative connotation - in that they may be used by advertisers to record information about a user's web habits. On the other hand, they are also often used so that a user does not have to constantly reenter their login credentials everytime a user visits a site. 

You can see what cookies are sent with requests by navigating to your (in chrome) developer tools and opening the `network tab`. Refreshing your page will display a list of incoming resources, of which you can select one and view the `headers` for the page. Scroll through the list and see if you find any cookies!

### [Sessions](https://en.wikipedia.org/wiki/Session_(web_analytics)) / [Session Management](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet)

Rather than attempt to begin with my simplistic attempt at describing Sessions, I will quote from the OWASP session management cheat sheet:

> A web session is a sequence of network HTTP request and response transactions associated to the same user. Modern and complex web applications require the retaining of information or status about each user for the duration of multiple requests. Therefore, sessions provide the ability to establish variables – such as access rights and localization settings – which will apply to each and every interaction a user has with the web application for the duration of the session.

A step-by-step, general example:

- Beorn goes to `http://knittingworld.com` to get some nice yarn.
- When Beorn logs in, he is sending his credentials to a server.
- When the credentials reach the server, the server, in one way or another, needs to check if Beorn is a user in their database. At this point, Beorn is not yet logged in.
- The server looks in the database to see if Beorn's username and password match a record in the database. It does! Excellent, things are looking good!
- Note: This is where things get a bit tricky. HTTP is _stateless_ ... that means that there is no persistent data that sticks around in HTTP requests — so right now, although Beorn is "in the middle of logging in" — there's nothing (yet) to identify him as "logged in" on future requests. He needs something to _identify_ him on future requests to the server—especially if he's trying to buy things that require him to be logged in. This is where the idea of authentication sessions come in:
- Now that the server knows who Beorn is, and has identified him as a user proper in the database, the server will send him (or "return" ) a special "token" or "session id" — that can identify Beorn as someone who is "logged in" on future requests. 
- Note: the return "token" or "session id" has commonly been in the form of cookies — session cookies — but it is becoming more common to use token's (such as JSON Web Tokens)
- When Beorn logs out from `http://knittingworld.com`, his session id will expire (so that next time he visits, he will have to log in again).


More links on sessions:

[How does a web session work](http://machinesaredigging.com/2013/10/29/how-does-a-web-session-work/)

[What are web sessions?](http://stackoverflow.com/questions/3804209/what-are-sessions-how-do-they-work)


# Methodologies

It's time to get into the actual body of text on writing authentication. Following is a list of technologies for establishing an authentication solution. This is not an exhaustic list.

## Technologies

### [HTTP Basic Authentication](https://en.wikipedia.org/wiki/Basic_access_authentication)

HTTP Basic authentication ( or "Basic Auth") has been around for quite some time. It seems that people tend to use it for it's simplicity and it's wide support across browsers. Here's how it would work in the case of our friend Beorn.

- Beorn goes to `http://knittingworld.com` to get some nice yarn.
- Beorn types in his username and password into a login _form_. This information gets `POST`ed to the server using an XML http request (although you might be more familiar with the term `ajax`  or, `fetch` request).
- The POST request from the client contains _headers_ that have the authorization information (username and password) attached. This header might look something like this: 

`Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`

- The server goes on to validate the authentication header and determine if a user can proceed to a restricted route or not.

Some important notes on HTTP Basic Authentication

- The example authorization header above does not look like a username and password, but that is because it is base64 encoded. IT IS NOT ENCRYPTED.
- USE HTTPS. If you just use HTTP, authentication credentials are sent to a server as PLAIN TEXT. A user's username and password are being sent over the wire merely as base64 encoded text — which is trivial to decode. By using HTTPS / SSL you are ensuring that data sent from the client to the server is encrypted.
- The use of HTTPS / SSL might Basic Auth a viable method for your authentication solution but there are still some caveats. 
- There needs to be _some_ sort of caching of a username / password, _because_ HTTP requests are stateless; credentials _have_ to be sent to the server on every request!
- Where are you going to cache a user's password + username? Will it be visible in a cookie? Is that secure? You could perhaps run an encryption algorithm on credentials and store it as a cookie, but it's still public visible in your browser. Continuing on...
- When the password arrives at the server, it will be hashed by your encryption algorithm (which you absolutely should have ) to see if it matches the password in the database. Some people argue that the processing power to encrypt passwords / check on every request is not efficient. I have no say in this matter because I don't know. But consider that idea as you look into other authentication methods.


Links

Some of these links are opinionated. I did not find many articles discussing Basic Auth, although it seems to still be a viable option for companies today. 

[Basic Authentication on OWASP](https://www.owasp.org/index.php/Basic_Authentication)

[Why does stripe use HTTP basic auth with a token instead of a header?](https://www.quora.com/Why-does-Stripe-use-HTTP-Basic-Auth-with-a-token-instead-of-a-header?share=1)

[Why the hell does your api still use basic auth?](http://swaggadocio.com/post/48223179207/why-the-hell-does-your-api-still-use-http-basic)

### HTTP digital access authentication

### Session based Authentication

### OAuth

### OpenId

### Token based Authentication

### JWT


# Resources and Footnotes

[1 - Dos and Don’ts of Client Authentication on the Web](https://pdos.csail.mit.edu/papers/webauth:sec10.pdf). Page 2.

[2 - Dos and Don’ts of Client Authentication on the Web](https://pdos.csail.mit.edu/papers/webauth:sec10.pdf). Page 1.

http://stackoverflow.com/questions/549/the-definitive-guide-to-form-based-website-authentication

https://blog.codinghorror.com/youre-probably-storing-passwords-incorrectly/

https://pdos.csail.mit.edu/papers/webauth:sec10.pdf

[OWASP Authentication Cheat Sheet
](https://www.owasp.org/index.php/Authentication_Cheat_Sheet)
