# Auth-Boss
Become an Auth Boss. Learn about different authentication methodologies on the web.

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
  - [HTTP](#http)
  - [HTTPS](#https)
  - [TLS / SSL](#tls--ssl)
  - [State](#state)
  - [Cookies](#cookies)
  - [Sessions / Session Management](#sessions--session-management)
- [Methodologies](#methodologies)
  - [HTTP Basic Authentication](#http-basic-authentication)
  - [Session based Authentication](#session-based-authentication)
  - [Token based Authentication](#token-based-authentication)
  - [JWT](#jwt)
  - [OAuth](#oauth)
  - [OpenId](#openid)
- [Resources and Footnotes](#resources-and-footnotes)
- [Thank-You](#thank-you)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
# Introduction

The intention of this document is to chronicle and catalog the methodologies of _authentication_ on the web. By _authentication_ I am referring to the process of creating a system through which users can "login" to an online service and be given access to otherwise protected resources.

The following quote might better sum up what I'm trying to cover:

> Client authentication involves proving the identity of a client (or user) to a server on the Web.[1]

***

## Who
I am a self-taught developer with a passion for open source technology, learning, mentoring, and knowledge-sharing.

## Why
I am writing this guide because I felt woefully at a loss for finding resources that explained safe, smart and current technologies and methods for authentication in a simple and concise manner. I decided it was time to put my research hat on and do some leg work.

## How
My writing style aims to be simple, terse, and to leverage layperson terms over jargon and tech-words.

**Disclaimer** : This document does not serve as an all-encompassing catalog of all authentications methods for the web; nor does this document purport to provide the "best" methods of authentication. Nobody is paying me to display the links that I have chosen to display. If you want to pay me money for being awesome, just like... pay me some money some other way. Or go pet a puppy, or help someone carry their groceries if you see them struggling. I don't know.

**Regarding Quotes / Citations**: I have _very_ informally cited my sources. If you want me to take down links / better cite my sources (beyond just a link to original posting) I can do that. Just let me know <3.

**Regarding mistakes, errors, and inconsistencies**: I am bound to make mistakes and miss many things. I AM NOT A SECURITY EXPERT. If you see something that could be improved make a pull request explaining what I missed + what I can improve. Bonus points if you're not a jerk about it. Check out the `CONTRIBUTIONS.md` for more information on PRs and junk.


# Examples cases

I will use a common example across this document to illustrate a login flow both in terms of what is happening on the "client" (the user in front of their computer) and what is occurring on the "server" (behind the scenes, so to speak).

Our example cases will feature a new imaginary friend: `Beorn`. Beorn likes knitting and often goes to `http://knittingworld.com` to buy supplies. Beorn has a user account on `knittingworld` and we will see him continuously login across the examples in this document.


# General Best Practices

Before jumping into the technologies that exist for managing authentication, let's consider some best practices/things you should never do.

Some of these following items may not directly pertain to login/authentication/user signups but are generally useful and good to know.

- Never store passwords as plain text in a database.
- Never write your own hashing algorithm (unless you're really really smart)
- Do not write your own authentication technology (again, unless you're really really smart).
- Use [HTTPS](https://en.wikipedia.org/wiki/HTTPS).

**Some quotes**

> We also found that many Web sites would design their own authentication mechanism to provide a better user experience. Unfortunately, designers and implementers often do not have a background in security and, as a result, do not have a good understanding of the tools at their disposal[2]


# Terminology

There is quite a bit of jargon in this sub-set of web development. Below is a list of terms that you will see again and again when it comes to authentication. Each word will link to a definition that I have found, but the descriptions written here are my attempts to create a concise summary in lay-person terms.


## [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)

Stands for Hyper Text Transfer Protocol. This is a big concept, and I'm not sure I can do it justice to explain simply without being reductive. The web is built around HTTP - it's the technology used to communicate between web servers and users.

Your browser is considered an HTTP client because it sends _requests_ to an HTTP server. There are many different kinds of _requests_ that your client can make — you may have heard of some of the most popular ones — `GET` `POST` `PUT` and `DELETE`.

HTTP servers send _responses_ to your browser — the client. These responses come in the form of _resources_. Resources could be (but are not limited to): HTML files, images, text, JSON, and more. You can (kind of) think of _resources_ as "files" that come back from a server.

_Other links on this topic_:

- [HTTP Made Really Easy](http://www.jmarshall.com/easy/http/#whatis)
- [RFC2616](https://tools.ietf.org/html/rfc2616) — this is a massive document-spec-thing on HTTP. It is listed as outdated, but also lists the documents that have superseded it.


## [HTTPS](https://en.wikipedia.org/wiki/HTTPS)

HTTPS is HTTP with more security. It goes hand in hand with `SSL / TLS`. Originally made popular for payment transactions over the Internet, it has become much more widespread lately. You may recognize https as "that green text that shows up to the left of my URL in the browser"; often accompanied by an icon of a lock or something of the sort.

HTTPS is HTTP wrapped in TLS (or, in the olden days, SSL) to protect traffic between browsers and servers.

HTTPS encrypts the information that's sent along with your HTTP requests and the responses that are sent back. This is particularly important when we start talking about authentication!

From Wikipedia:

> HTTPS creates a secure channel over an insecure network. This ensures reasonable protection from eavesdroppers and man-in-the-middle attacks, provided that adequate cipher suites are used and that the server certificate is verified and trusted.


## [TLS / SSL](https://en.wikipedia.org/wiki/Transport_Layer_Security)

TLS and SSL are cryptographic protocols. TLS and SSL encrypt the data you send across a network — it is designed to prevent people from "eavesdropping" or tampering with the data that you are sending.

SSLv2 and v3 are considered insecure today (see [POODLE](https://en.wikipedia.org/wiki/POODLE)), so most HTTPS is done with TLS 1.2.

There are some useful videos on youtube that help to explain some of these complex topics, but unfortunately, I can't speak to the veracity of them all. This [video by MIT opencourseware](https://www.youtube.com/watch?v=S2iBR2ZlZf0) looks relatively useful!


## [State](https://en.wikipedia.org/wiki/State_(computer_science))

This document will make reference to the terms `state`, `stateful`, `stateless` and `piece of state`. These are broad terms that vary in their definition. For the sake of this article, a "piece of state" or something that is "stateful" is describing a piece of data that is living in memory...somewhere.

HTTP requests are commonly described as "stateless". When you visit a website and login, you are passing some information along with your HTTP request — something that _identifies you_. Whatever method of authentication you need to use to identify yourself has to be "attached" to HTTP requests in some way or another, _because_ you cannot simply store that state within the HTTP protocol itself — it has to take another form that can "ride along" with your HTTP requests, so to speak (as you will see in the rest of this documentation.).

To perhaps overstate / over explain ... I think that this quote from the Scotch.io article is quite useful [The Ins and Outs of Token Based Authentication](https://scotch.io/tutorials/the-ins-and-outs-of-token-based-authentication)

> Since the HTTP protocol is stateless, this means that if we authenticate a user with a username and password, then on the next request, our application won’t know who we are. We would have to authenticate again.


## [Cookies](https://en.wikipedia.org/wiki/HTTP_cookie)

Cookies, at the very base level, are small pieces of data that get stored on a _user's browser_. Cookies, in contrast to HTTP, are _stateful_ — meaning that while HTTP cannot store user information, cookies can.

A common example of a web cookie:

Beorn visits `http://knittingworld.com` to buy some nice yarn and materials for his next knitting project. He logs in and adds three items to his cart. As he is about to "check out" in the online store and ship the items to his home, he hears a loud "bang!" and realizes that he left a can of tuna in the microwave on high. Not good! Beorn closes the browser, instantly forgetting about his new knitting materials, and goes to clean up the mess. After the smell of canned tuna has finally sunk into the walls and carpet of his home, Beorn returns to his computer and revisits `https://knittingworld.com` ... only to find his items are still in his cart. How?! Cookies.

There are different kinds of cookies. Some cookies will stick around in your browser for many days, while others will disappear as soon as you close your browser.

Cookies have played a big role in authentication in the past (and still do). Authentication cookies are commonly used by web servers to determine whether a user is logged in or not and what resources they have access to.

[Persistent cookies](https://en.wikipedia.org/wiki/HTTP_cookie#Persistent_cookie) sometimes carry a negative connotation, in that they may be used by advertisers to record information about a user's web habits. On the other hand, they are often used to save a user from having to re-enter their login credentials every time they visit a site.

You can see what cookies are sent with requests by navigating to your (using Chrome, for example) developer tools and opening the `Network` tab. Refreshing your page will display a list of incoming resources, of which you can select one and view the `headers` for the page. Scroll through the list and see if you find any cookies!

You can also view cookies/storage related information in the "Application" tab in your developer tools.

## [Sessions](https://en.wikipedia.org/wiki/Session_(web_analytics)) / [Session Management](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet)

Rather than attempt to begin with my simplistic attempt at describing sessions, I will quote from the OWASP session management cheat sheet:

> A web session is a sequence of network HTTP request and response transactions associated with the same user. Modern and complex web applications require the retaining of information or status about each user for the duration of multiple requests. Therefore, sessions provide the ability to establish variables – such as access rights and localization settings – which will apply to each and every interaction a user has with the web application for the duration of the session.

You can find a step-by-step example of session-based authentication in the `Methodologies` section below.

More links on sessions:

- [How does a web session work](http://machinesaredigging.com/2013/10/29/how-does-a-web-session-work/)
- [What are web sessions?](http://stackoverflow.com/questions/3804209/what-are-sessions-how-do-they-work)


# Methodologies

It's time to get into actual examples of authentication methods!. Following is a list of technologies for establishing an authentication solution. This is not an exhaustive list!


## [HTTP Basic Authentication](https://en.wikipedia.org/wiki/Basic_access_authentication)

HTTP Basic authentication (or "Basic Auth") has been around for quite some time. It seems that people tend to use it for its simplicity and its wide support across browsers. [Here is a blank page](https://httpbin.org/basic-auth/user/passwd) that asks for Basic auth. Here's how it would work in the case of our friend Beorn:

- Beorn goes to `http://knittingworld.com` to get some nice yarn.
- After picking out his yarn, he clicks "Buy" to buy it.
- His browser makes a GET request and the server responds that he needs to authenticate (by sending a `401 Unauthorized` status and a `WWW-Authenticate` header).
- Beorn types in his username and password into a login form.
- His browser makes the GET request with an `Authorization` header. This header might look something like this:

`Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==`

- The server goes on to validate the authentication header and determine if Beorn can proceed to his shopping cart. The browser remembers the credentials and resends them on that domain until it is closed.

**Some important notes on HTTP Basic Authentication:**

- The example authorization header above does not look like a username and password, but that is because it is base64 encoded. IT IS NOT ENCRYPTED.
- USE HTTPS if you are using Basic Auth. If you just use HTTP, authentication credentials are sent to a server as PLAIN TEXT. THIS IS BAD. A user's username and password are being sent over the wire merely as base64 encoded text — which is trivial to decode. By using HTTPS / TLS, you are ensuring that data sent from the client to the server is encrypted.
- Basic Auth, _as implemented by the browser_, is not exactly aesthetically pleasing and is rarely used today.
- Basic Auth with APIs, when combined with tokens, (discussed later) is just an `Authorization` header and is perfectly reasonable. It has the added benefit of not requiring the API client to maintain an extra session cookie and, since most systems, log query params but not headers, will not be logged by default.

**Links**

- [Basic Authentication on OWASP](https://www.owasp.org/index.php/Basic_Authentication)

- [Why does stripe use HTTP basic auth with a token instead of a header?](https://www.quora.com/Why-does-Stripe-use-HTTP-Basic-Auth-with-a-token-instead-of-a-header?share=1)


## [Session based Authentication](https://en.wikipedia.org/wiki/Session_(web_analytics))

Session authentication has been around for a while and is commonly practiced. The key component to Session based authentication is that a user's login is associated with a _piece of state_ either _in memory_ on the server, or in a key-value store (like Redis).

Let's look at an example of our friend Beorn using session based authentication.

- Beorn goes to `http://knittingworld.com` to get some nice yarn.
- When Beorn logs in, he is sending his credentials to a server.
- When the credentials reach the server, the server, in one way or another, needs to check if Beorn is a user in their database. At this point, Beorn is not yet logged in.
- Beorn's credentials checkout, so he can log in.
- Beorn needs something to _identify_ him on future requests to the server — especially if he's trying to buy things that require him to be logged in. This is where the idea of authenticated sessions come in.
- Now that the server knows who Beorn is, and has identified him as a user in the database, the server will send him (or "return") a cookie — that can identify Beorn as someone who is "logged in" on future requests.
- Now that Beorn is authenticated and has a session cookie on his browser, he can go check out the yarn that is on sale for logged in members only.
- When Beorn goes to the page `http://knittingworld.com/great_deals.html` he is making yet another HTTP request - but this time, his session cookie will ride along with the HTTP request to the server.
- the server will authenticate based on the cookie matching the in-memory session information (saving the need to run to the database, check passwords, etc. — like in Basic Auth)
- When Beorn logs out from `http://knittingworld.com`, his session instance on the server (or on Redis, etc.) will expire and so will his session cookie.


## [Token based Authentication](http://stackoverflow.com/questions/1592534/what-is-token-based-authentication)

Token based authentication has become more popular lately with the rise of RESTful APIs, single-page-apps, and micro-services.

**What is a token?**

A token is a small piece of data.

An authentication system that leverages token-based-authentication means that the requests a user makes to a server carry a token along with them to perform authentication logic on. When HTTP requests are made, the token is the piece of data that verifies a user's eligibility to access a resource.

**How is this different from cookie-based authentication?**

Token authentication is stateless, whereas session based authentication means that somewhere on your server (or in Redis etc.) you have a piece of state that is keeping track of user sessions.

Auth0's blog post [Cookies vs. Tokens: The Definitive Guide](https://auth0.com/blog/cookies-vs-tokens-definitive-guide/) draws out a great step-by-step comparison of the difference in authentication flows between cookies and tokens (so, that means Beorn shall be missing from this example section):

**Session based authentication flow**:

```
1. User enters their login credentials
2. Server verifies the credentials are correct and creates a session which is then stored in a database
3. A cookie with the session ID is placed in the user's browser
4. On subsequent requests, the session ID is verified against the database and if valid the request processed
5. Once a user logs out of the app, the session is destroyed both client and server side
```

**Token based authentication flow**:

```
1. User enters their login credentials
2. Server verifies the credentials are correct and returns a signed token
3. This token is stored client-side, most commonly in local storage - but can be stored in session storage or a cookie as well
4. Subsequent requests to the server include this token as an additional Authorization header or through one of the other methods mentioned above
5. The server decodes the JWT and if the token is valid processes the request
6. Once a user logs out, the token is destroyed client-side, no interaction with the server is necessary
```

A key takeaway is that tokens are **stateless**. A back-end server doesn't need to keep a record of tokens, or of currently active sessions.

**Wow tokens sound cool. Are they better than session based auth?**

You're asking the wrong person, pal. I'm just writing this to help my brain figure things out. Like most things on the internet, there are lots of people with strong opinions one way or another. I'm doing my best to make it the case that you won't find those here (Although many of the links I've posted from my research are full of strong opinions or are financially biased).

For more fun disclaimers visit the `disclaimer` section above.

**Types of Tokens**
Some common tokens include JWT (discussed below), SWT (Simple web tokens) and SAML (security assertion markup languages)

**Links**

- [Token Based Authentication - Implementation Demonstration - W3](https://www.w3.org/2001/sw/Europe/events/foaf-galway/papers/fp/token_based_authentication/)
- [What is token based Authentication - SO](http://stackoverflow.com/questions/1592534/what-is-token-based-authentication)
- [Token Based Authentication Made Easy](https://auth0.com/learn/token-based-authentication-made-easy/#!)
- [The Ins and Outs of Token Based Authentication](https://scotch.io/tutorials/the-ins-and-outs-of-token-based-authentication)
- [Cookies vs Tokens: The Definitive Guide (opinionated)](https://auth0.com/blog/cookies-vs-tokens-definitive-guide/)


## [JWT](https://en.wikipedia.org/wiki/JSON_Web_Token)

JWT stands for "JSON Web Token". JWT is a **type** of token-based authentication. JWT is based on a [web standard](https://tools.ietf.org/html/rfc7519). If you are already asking yourself/me if that makes it "better" than Token based authentication, let's table that question for another time / for people who are smarter than me. I am including a write up for JWT's here because they seem to be becoming much more common; even though they are a "subset" so to speak of the above section on "token based authentication". Once again, different methodologies of token based authentication have different pros and cons. 

So, a lot of the information in the `token based authentication` section above applies here.

The abstract from the JWT RFC 7519 standardization states that:

>JSON Web Token (JWT) is a compact, URL-safe means of representing claims to be transferred between two parties.

A JSON Web Token is a string of characters. It might look like this:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

The above string is what you might see when you are performing authentication using JWT; it is the encoded data that is returned from a server upon authentication.

JWT's are verified and made secure because they are "digitally signed" with a private-key, and authenticated with a secret key.

**What makes up a JWT?**

A JWT is a self-contained chunk of data. Every JWT is made up of a `header` a `payload` and a `signature`. When your server creates a token, you can also assign unique data to your token, which can be used on the front end. This can be used to save the need for making other database calls later.

You should still be wary of posting confidential information in a token sent to a client.

**Example function to create a JWT token in Python**:

```python
def create_token(user):
    """Create a JWT token, set expiry, iat, etc"""
    payload = {
        'sub': user.id,
        'name': user.first_name,
        'role_id': user.role_id,
        'iat': datetime.utcnow(),
        'exp': datetime.utcnow() + timedelta(days=1)
    }
    token = jwt.encode(payload, MY_SECRET_KEY_SHHH_DONT_TELL_ANYONE, algorithm='HS256')
    return token.decode('unicode_escape')
```

The above keys `sub`, `iat` and `exp` follow the reserved JWT keys, but I've also added the `name` of the user and what `role_id` they have. You will need to get a library for encoding/decoding JWT tokens. [JWT.io](https://jwt.io/) lists lots of libraries for many languages.

In some cases, a JWT secret key may be base64 encoded — in which case, depending on your JWT library — you need to decode the secret key prior to decoding the token. 

**Links**

- [Introduction to JSON web tokens](https://jwt.io/introduction/)
- [JWT Debugger](https://jwt.io/)


## [OAuth](https://en.wikipedia.org/wiki/OAuth)

OAuth is an _authorization_ protocol that allows users to perform an authentication against a server without a password. OAuth has seen various iterations — OAuth 1.0, OAuth 1.0a, and OAuth 2.0.

At this point we could have a very pedantic conversation about Authentication and Authorization. But we shan't! Instead, the important thing to note is the following (thanks [@rohan](https://github.com/rohannair))

- `Authentication` - Proving who you are.

- `Authorization` - Determining what you have permission to do.

This is a whole can of worms in itself. The OAuth docs state the following, plain and clear in their [Authentication](https://oauth.net/articles/authentication/) article:

> OAuth 2.0 is not an authentication protocol.

With that said, there is still good reason to discuss OAuth. In fact, the above article is quite well written, so I'm going to quote it here:

> Authentication in the context of a user accessing an application tells an application who the current user is and whether or not they're present. A full authentication protocol will probably also tell you a number of attributes about this user, such as a unique identifier, an email address, and what to call them when the application says "Good Morning". Authentication is all about the user and their presence with the application, and an internet-scale authentication protocol needs to be able to do this across network and security boundaries.

> However, OAuth tells the application none of that. OAuth says absolutely nothing about the user, nor does it say how the user proved their presence or even if they're still there. As far as an OAuth client is concerned, it asked for a token, got a token, and eventually used that token to access some API. It doesn't know anything about who authorized the application or if there was even a user there at all. In fact, much of the point of OAuth is about giving this delegated access for use in situations where the user is not present on the connection between the client and the resource being accessed. This is great for client authorization, but it's really bad for authentication where the whole point is figuring out if the user is there or not (and who they are).

If you have ever logged into a service by using your Twitter, Google, or Facebook account, then you have used OAuth.

OAuth Providers (Facebook, Google, etc), operate through private, unique, access tokens that provide the means of authentication for your service (the "OAuth client") to allow logins.

If you want to use OAuth for your users to log into your service, you will need to register your server as an OAuth Client. This will usually set you up with a `client id`, and `client secret`. Users that log in to your service will be relocated to the OAuth Provider where the user can confirm that they do indeed want to "log in" (i.e. allow the server they are logging into, to have access to any required information from the OAuth Provider. )

In the case of our friend Beorn...

- Beorn goes to `http://knittingworld.com` to get some nice yarn.
- Beorn decides to log in using his Google account.
- Beorn is prompted to enter his Google account credentials (in the case that he isn't already logged in)
- After entering his credentials, Google (or whatever OAuth provider he uses) will prompt him to check that he wants to sign into `http://knittinggworld.com` with his Google account.
- After accepting, Beorn is redirected to `http://knittingworld.com`.
- If `knittingworld` needs access to a resource regarding Beorn's information, it can make requests to a `resource server` (via the OAuth provider) to access them, provided its access token is valid.


From the [OWASP Authentication Cheat Sheet](https://www.owasp.org/index.php/Authentication_Cheat_Sheet#OAuth)
>The recommendation is to use and implement OAuth 1.0a or OAuth 2.0 since the very first version (OAuth1.0) has been found to be vulnerable to session fixation.

> OAuth 2.0 relies on HTTPS for security and is currently used and implemented by APIs from companies such as Facebook, Google, Twitter, and Microsoft. OAuth1.0a is harder to use because it requires the use of cryptographic libraries for digital signatures. However, since OAuth1.0a does not rely on HTTPS for security, it can be more suited for higher risk transactions.

**Links**

- [A Fun explanation of OAuth involving Donuts](http://stackoverflow.com/a/32534239)


## [OpenId](https://en.wikipedia.org/wiki/OpenID)

OpenId is another authentication protocol that (like OAuth) does not require a password. The OpenId [website](http://openid.net/get-an-openid/what-is-openid/) has a very succinct and clear description, in my opinion:

> OpenID allows you to use an existing account to sign in to multiple websites, without needing to create new passwords.

> You may choose to associate information with your OpenID that can be shared with the websites you visit, such as a name or email address. With OpenID, you control how much of that information is shared with the websites you visit.

> With OpenID, your password is only given to your identity provider, and that provider then confirms your identity to the websites you visit.  Other than your provider, no website ever sees your password, so you don’t need to worry about an unscrupulous or insecure website compromising your identity.

Although started in 2005, OpenId published `OpenId Connect` in 2014 which is an "interoperable authentication protocol based on the OAuth 2.0 family of specifications" ([source](http://openid.net/connect/faq/))

**What's the Difference between OpenId and OAuth?**

OpenId is similar to OAuth, but there are some differences. Similarly, OpenId relies on an `Identity Provider` that interacts with third parties -- `relying parties` (the site you are logging into) to provide authentication credentials.

Dissimilarly, you might use OAuth to allow the site you are logging into to have access to your data from the Provider. That might sound scary and confusing but here's a simple example:

- Beorn signs up for Twitter. He is going to tweet pictures of the sweet hat he's knitting.
- Beorn doesn't know whom to follow, and nobody is following him. Beorn is sad and feels unimportant.
- Twitter prompts Beorn to use OAuth to connect his Google account so that he can import his contacts that also have Twitter.
- Beorn follows a bunch of people, including old friends from high school he hasn't seen in many years.
- Beorn does this, and now he is tweeting non-stop.

**Links**

- [What is OpenId?](http://openid.net/get-an-openid/what-is-openid/)
- [OpenId Connect FAQ](http://openid.net/connect/faq/)
- [OpenId Connect Video](https://www.youtube.com/watch?v=Kb56GzQ2pSk)
- [What's the differene between OpenId vs OAuth?](http://stackoverflow.com/questions/3376141/openid-vs-oauth?rq=1)
- [What's the differene between OpenId vs OAuth? (different thread)](http://stackoverflow.com/questions/1087031/whats-the-difference-between-openid-and-oauth)
- [OpenId according to Dave -- I like this one, albeit dated](https://www.youtube.com/watch?v=xcmY8Pk-qEk)

***

Here's an image of Stack Overflow's login page, which offers many different authentication methods:

![](images/SO_login.png)


# Resources and Footnotes

[1 - Dos and Don’ts of Client Authentication on the Web](https://pdos.csail.mit.edu/papers/webauth:sec10.pdf). Page 2.

[2 - Dos and Don’ts of Client Authentication on the Web](https://pdos.csail.mit.edu/papers/webauth:sec10.pdf). Page 1.

http://stackoverflow.com/questions/549/the-definitive-guide-to-form-based-website-authentication

https://blog.codinghorror.com/youre-probably-storing-passwords-incorrectly/

https://pdos.csail.mit.edu/papers/webauth:sec10.pdf

[OWASP Authentication Cheat Sheet
](https://www.owasp.org/index.php/Authentication_Cheat_Sheet)

# Thank-You
Thank you to the kind people who put up with my million++ questions.

- [@neil](https://github.com/nchudleigh)
- [@Alex-wilmer](https://github.com/alex-wilmer)
- [@AdrianLi](https://github.com/adrianmcli)
- [@Rohan](https://github.com/rohannair)
