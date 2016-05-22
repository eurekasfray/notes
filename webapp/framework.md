### Preamble

In this document I write what I know about web frameworks, because I can't find any detailed descriptions of how web frameworks work. I had to read the sources. That said, I'd like to change this by providing notes on the fundamentals of web frameworks.

Currently web frameworks are a bit of a mystery to me, so I document what I know. I'm still learning and I'll update this doc as I continue. Free to pull me up on any anything that's incorrect. May this doc be valuable to those wanting to learn about web frameworks.

> Draft: 20160522


# What is a web framework?

A web framework is a library of code, created to help ease web development by providing common functionalities found in a webapp.


# Responsibilities

- Inspect the request and return the appropriate page
- Handle sessions and cookies
- Provide security


# Operation

- At its foundation a framework must fascillitate the communication between the client and server. The client and server communicate using the request-response pattern. The client requests a piece of data and the server responds with the requested data. The role of the framework is to inspect the request and return the appropriate response.

- How do we allow the programmer to map responses to requests? To accomplish this, the framework must allow the developer to specify the request and the response.


# Features

- URL mapping
- Templating
- Push-based MVC (Model-View-Controller)
- Request-response pattern
- Session management
- Database abstraction layer


# Components

## Front controller

## Router

## Kernel

## Controller

## View

## Model
