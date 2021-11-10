---
title: Introduction of the Entity Framework
tags: .Netcore/API/MVC
author: Anning Mao
---



## The Concept of EF

Before .NET3.5, we often wrote ADO.NET code or enterprise data access blocks to save or retrieve data in the underlying database. The method is: **open a database connection, create a DataSet to get or submit data to the database, and meet business needs by converting the data in the DataSet and the .NET objects to each other.** This is a cumbersome and error-prone process. Microsoft provides the "Entity Framework" framework to automatically perform all the above-mentioned database-related activities.
EF is an open source ORM framework suitable for .NET development. **It enables developers to process data through domain objects without having to pay attention to the underlying database that stores this data** . Using the Entity Framework, developers can work at a higher level of abstraction when processing data, and can use less code to create and maintain data-oriented applications compared to traditional applications.
Official definition: "Entity Framework is an object-relational mapper (O/RM) that enables .NET developers to manipulate databases through .NET objects. It eliminates the need for most data access codes that developers usually need to write ."

## Feature of EF 

1. **Cross-platform** EF core is a cross-platform framework that can run on Windows, Linux, and Mac
2. **Modeling** EF can create EDM(Entity Data Model) with different data types, and it uses this model to query or save data in the underlying database.
3.  **Query** EF allows us to use LINQ to retrieve data from the underlying database, and it also support direct execution of raw SQL queries on the database.
4. **Change tracking** EF tracks the changes of entity instance (property values) that need to be submitted to database
5. **Save** EF calls the SaveChanges () method, according to the changes in the entity, execute INSERT , UPDATE and DELETE commands on the database . EF also provides an asynchronous SaveChangesAsync () method.
6. **Concurrent** By default, data is extracted from the start from the database, EF using optimistic lock to avoid changes we make is covered by other users.
7. **Transaction** EF automatically performs transaction management when querying or saving data. It also provides options for custom transaction management.
8. **Cache**EF uses the level 1 cache. Therefore, repeated queries will return data from the cache instead of accessing the database.
9. **Configure** EF allows us to use annotations configuration properties EF model, you can also use the Fluent API to override the default convention.
10. **Migration** EF provides a set of migration command, We can execute these commands in the NuGet Package Manager console or command line interface to create or manage the underlying database.



## Composition of EF

The composition of EF is briefly summarized as follows:

![1.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/1.3%20Entity%20Framework/1.1.png)

**1. EDM (Entity Data Model):** EDM consists of three main parts: conceptual model, mapping and storage model.

**Conceptual model (entity):** The conceptual model contains model classes and the relationships between   them. This will be independent of the database table design.

**Storage model (data):** The storage model is a database design model, including tables, views, stored procedures, and the relationships and keys between them.

**Mapping:** Mapping consists of information about how the conceptual model is mapped to the storage model.

**2. LINQ To Entity (L2E):** L2E is a language for querying entity objects, which returns the entities defined in the conceptual model. 

**3. Entity SQL:** Entity SQL is a query language similar to L2E. However, it is more complicated than L2E.

**4. Object Services:** Object services are the main entry for accessing and returning the data. It is responsible for data instantiation and converts the data of the **Entity Client Data Provider** (the next layer) into entity objects.

**5. Entity Client Data Provider:**  The main responsibility is to convert L2E or Entity Sql into SQL query statements that the database can recognize. It sends or requests data to the database through the **ADO.Net Data Provider** .

**6. ADO.Net Data Provider:** Use standard Ado.net to communicate with the database.