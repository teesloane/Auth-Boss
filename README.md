
# Auth-Boss
Become an Auth Boss. Learn about different Auth methodologies.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents** 

- [Introduction](#introduction)
  - [Who:](#who)
  - [Why:](#why)
  - [How:](#how)
- [Examples cases](#examples-cases)
- [General Best Practices](#general-best-practices)
- [Terminology](#terminology)
  - [Terms](#terms)
- [Methodologies](#methodologies)
  - [Technologies](#technologies)
- [Resources and Footnotes](#resources-and-footnotes)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
# Introduction

The intention of this document is to chronicle and catalogue the methodologies of _authentication_ on the web. By _authentication_ I am referring to, in the simplest sense, the process of creating a system through which user's can "login" to an online service and be given access to otherwise protected resources.

This quote might better sum up what I'm trying to cover:

> Client authentication involves proving the identity of a client (or user) to a server on the Web. We will use the term “authentication” to refer to this problem.[1]

***

## Who:
I am a self-taught developer with a passion for open source tech, learning, mentoring, and knowledge-sharing. 

## Why:
I am writing this guide, because I felt woefully at a loss for finding resources that explained safe, smart and current technologies and methods for authentication in a simple and concise manner. I decided it was time to put my research hat on and do some leg work. 

## How:
My writing style aims to be simple, terse, and to leverage layperson terms over jargon and tech-words. 

**Disclaimer** : This document does not serve as an all-encompassing catalogue of all authentications methods for the web; nor does this document purport to provide the "best" methods of authentication.

**Regarding mistakes, errors, and inconsistencies**: I am bound to make mistakes and miss many things. I AM NOT A SECURITY EXPERT. If you see something that could be improved make a pull request explaining what I missed + what I can improve. Bonus points if you're not a jerk about it.

# Examples cases  

I will use a single example across this document to illustrate a login flow both in terms of what is happening on the "client" (the user in front of their computer) and what is occurring on the "server" (behind the scenes, so to speak).

Our example cases will feature a new imaginary friend: `Beorn`. Beorn likes making quilts, and often goes to `http://www.quiltmadness.com` to buy quilting supplies. Beorn has a user account on `quiltmadness` and we will see him continuously login across the examples in this document. 

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

HTTP

Cookie

TLS

SSL

JWT - A type of token based authentication ... 

2 (3, 4) Factor authentication 

Sessions

# Methodologies

Following is a list of technologies for establishing an authentication solution.

## Technologies

Http BasicAuth

Session based Authentication

OAuth

OpenId

Token based Authentication

JWT


# Resources and Footnotes

[1 - Dos and Don’ts of Client Authentication on the Web](https://pdos.csail.mit.edu/papers/webauth:sec10.pdf). Page 2.

[2 - Dos and Don’ts of Client Authentication on the Web](https://pdos.csail.mit.edu/papers/webauth:sec10.pdf). Page 1.

http://stackoverflow.com/questions/549/the-definitive-guide-to-form-based-website-authentication

https://blog.codinghorror.com/youre-probably-storing-passwords-incorrectly/

https://pdos.csail.mit.edu/papers/webauth:sec10.pdf
