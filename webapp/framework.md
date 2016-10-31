### Preamble

In this document I write what I know about web frameworks. I provide notes on the fundamentals of web frameworks. Web frameworks are a bit of a mystery to me, so I document what I know. I'm still learning and I'll update this doc as I learn more. Free to pull me up on any anything that's incorrect. May this doc be valuable to those wanting to learn about web frameworks.

> Draft: 2016/05/22


# What is a web framework?

A web framework is a library of code, created to help ease web development by providing common functionalities found in a webapp.


# Responsibilities

- Inspect the request and return the appropriate resource
- Handle sessions and cookies
- Provide security


# Operation

- Fascillitate the communication between the client and server. The client and server communicate using the request-response pattern. The client requests a piece of data and the server responds with the requested data. The role of the framework is making inspecting the request and returning the appropriate response easy for the application.

- How do we allow the programmer to map responses to requests? The framework must allow the programmer to specify the request and the response.


# Architecture (draft)

- Front controller. [TBD]

- Some web frameworks are building on an MVC architecture. MVC allows the programmer to separate business logic from presentation logic. [TBD]

  - The Model plays a role of domain logic.
  - The View plays the role of presentation logic.
  - The Controller plays the role being mapped to a request, convert the request into commands for the model or view, and provice a response.
  - [TBD]


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
