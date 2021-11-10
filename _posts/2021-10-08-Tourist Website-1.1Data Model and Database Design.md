---
title: Data Model and Database Design
tags: .Netcore/API/MVC
author: Anning Mao
---



What is the first thing that needs to be done for a new project? Your answer may be "build a database". This is sufficient for small projects, but not enough for large projects.

A project should start from the business:

![1.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/1.1%20Database%20design/1.1.png)

## Summarize Businesses

The businesses included in our `Tourist Website` are as follows

![1.2](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/1.1%20Database%20design/1.2.png)

The businesses can be divided into 3 Module and this module-based design model is called Domain Driven Design(DDD)



## Build the Data Model

what is a Data Model?

Model is a way to describe complex things, it is a means to understand an object, a system, and a concept.

You can model a thing from multiple angles. For example: You can model apples separately from their appearance, chemical composition and taste and which angle to model from depends entirely on your needs(your businesses).

For a chemist, the chemical composition of an apple is the most suitable modeling direction. For a painter, appearance characteristics are a modeling direction; for a gourmet, taste is the most suitable modeling direction.



Back to my project. we should build the data model based on our three businesses module summarized in the last step:

 ![1.3](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/1.1%20Database%20design/1.3.png)



For an `Use` of a website, we don't care about his/her hair color or appearance, we only need the name, Phone number and Address. we finally get 6 entities and their attributes from 3 modules.	

## Entity-Relationship 

 ![1.4](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/1.1%20Database%20design/1.4.png)

One `Route` should have more than one display `picture` so the relationship is `1-n`

One `User` can have more than one ` Character` and one ` Character` include many `User`, so the relationship is `n-n`

...

## DataBase

![1.5](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/1.1%20Database%20design/1.5.png)

In the code implementation, the data model is that a class contains multiple attributes, these attributes can be mapped to the database, the programmer can manipulate the data in the form of objects

From a data point of view, the role of the data model is to obtain data, update data, transfer data, and save data. From the perspective of system responsibilities, the data model belongs to the business layer.

There is no strict definition of the model in the industry. Everyone uses the data model based on experience. The data model can be a simple POCO type or a complex rich domain model.



## How to get data from the database

You can JDBC, Ado.Net or ORM, but this project will use a mainstream data persistence model, repository. It  will help us use the objectified data without worrying about how the data is stored. Even the data storage format can be ignored. As long as we use the data mapping mechanism of the repository, whether you are using mysql, Oraclesql or even nosql, it can run perfectly.

To use the repository, we must first create some interfaces. In these interfaces, we will highly abstract the data persistence business. The specific steps of data acquisition, modification and saving will be hidden and encapsulated, and the encapsulated repository will provide the simplest API to execute Data manipulation

![1.6](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/1.1%20Database%20design/1.6.png)



## Benefits of using Data Models and Repository

- The business logic and the data model are precisely coupled to reduce layering and reduce the number of codes


- Completely divest the database business, programmers can focus more on the business logic


- Object-oriented programming, data is transformed into objects, more in line with human thinking

eg. 

to get the Zip code of some user with sql

```sql
SELECT ZipCode
FROM User
LEFT JOIN Address on user.Id= Address.UserId
WHERE User.Id=$id
```

same task with object oriented

``` 
User.Address.ZipCode
```

