---
title: Tourist website-1.1 Data model and database design
tags: .Net core/API/MVC
author: Anning Mao
---

# Data Model and Database Design

What is the first thing that needs to be done for a new project? Your answer may be "build a database". This is sufficient for small projects, but not enough for large projects.

A project should start from the business:

![1.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/Database%20design/1.1.png)



## Summarize Businesses

The businesses included in our `Tourist Website` are as follows

![1.2](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/Database%20design/1.2.png)

The businesses can be divided into 3 Module and this module-based design model is called Domain Driven Design(DDD)



## Build the Data Model

what is a Data Model?

Model is a way to describe complex things, it is a means to understand an object, a system, and a concept.

You can model a thing from multiple angles. For example: You can model apples separately from their appearance, chemical composition and taste and which angle to model from depends entirely on your needs(your businesses).

For a chemist, the chemical composition of an apple is the most suitable modeling direction. For a painter, appearance characteristics are a modeling direction; for a gourmet, taste is the most suitable modeling direction.



Back to my project. we should build the data model based on our three businesses module summarized in the last step:

 ![1.3](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/Database%20design/1.3.png)



For an `Use` of a website, we don't care about his/her hair color or appearance, we only need the name, Phone number and Address. we finally get 6 entities and their attributes from 3 modules.	

## Entity-Relationship 

 ![1.4](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/Database%20design/1.4.png)

One `Route` should have more than one display `picture` so the relationship is `1-n`

One `User` can have more than one ` Character` and one ` Character` include many `User`, so the relationship is `n-n`

...

## DataBase

![1.5](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/Database%20design/1.5.png)

All the Logic concept is clear and now we can build up our physical database.

