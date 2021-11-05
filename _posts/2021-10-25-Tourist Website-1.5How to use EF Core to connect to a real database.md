---
title: How to Use EF Core to Connect to A Real Database
tags: .Netcore/API/MVC
author: Anning Mao
---

## Build and setup the "Connector"

1. Install EntityFramework Core

   Project->Manage Nuget packages->Search `entityfamework core`

   ![1.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/5.How%20to%20use%20Entity%20Framework/1.1.png)

   

2. In order to configure the related functions of EF Core and access the database, we need to create a Database folder to store the AddDbContext file. This AddDbContext is a database mapping tool, which can ensure the data flow between the database and our code. In layman's terms, it is a connector.

   ![2.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/5.How%20to%20use%20Entity%20Framework/2.1.png)

   

3.  To build this "Connector", we should do as follows:

   - Add line `using Microsoft.EntityFrameworkCore;`

   - Let AddDbContext inherit DbContext in EF Core

   - Inject `DbContextOption` into Constructor, and the generic type should be `AddDbContext`

      we also need to call the base class (DbContext) and pass in `options`

   - In the context object, we need to specify which models need to be mapped to the database. In EF Core, we use DbSet to map models. Each data model must use a DbSet to map a database table.

   ![3.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/5.How%20to%20use%20Entity%20Framework/3.1.png)

   

4. Inject AddDbContext into the IOC 

   Before using DbContext, you need to configure it. The configuration is done in the parameter (option) of AddDbContext, so use the lambda expression call to configure the option

   ![4.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/5.How%20to%20use%20Entity%20Framework/4.1.png)

   

   For EF Core, we can configure the option through an extension framework

   ​	Project->Manage Nuget packages->Search `entityframeworkcore.sqlserver`

   ![4.3](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/5.How%20to%20use%20Entity%20Framework/4.3.png)

   

   From the description, we can know that this extension is a provider.

   So how to use this extension next? In fact, we don’t need to add a reference to this framework, just refer to EF Core and use `option.UseSqlServer()`. Because the framework is only extended for DbContext, as long as EF Core is referenced, the relevant extension package will be referenced at the same time . This means that if you do not want to use mssql server, but use other database services, you only need to install the corresponding official extension package written by Microsoft, so our project will not be coupled with the database, we can switch the database at will

   

   The option.UseSqlServer function accepts a connection string as a parameter

   For the Docker server we created in last article.

   ```c#
   option.UseSqlServer("server=localhost;Database=MyTourismSite; User ID=SA; Password=Man741852963.");
   ```

   For Visual studio built-in database, you can get the connection string from here

   ![4.4](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/5.How%20to%20use%20Entity%20Framework/4.4.png)

   

5. In a real project, we generally don’t write the connection string in our code, we usually put the connection string in the `appsettings.js` file, and then read it when needed

   - put connection in appsettings.json

     ![5.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/5.How%20to%20use%20Entity%20Framework/5.1.png)

   - Add this line`using Microsoft.Extensions.Configuration;`in Startup.cs. 

   - Create a private variable to store configuration information and  inject `configuration` into StartUp constructor.![5.2](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/5.How%20to%20use%20Entity%20Framework/5.2.png)

   then we can get the Connection String like following

   ```c#
   option.UseSqlServer(Configuration["DbContext:ConnectionString"]);
   ```

   

## Connect the repository to the real database

Now that we have configured the connector, we can connect our repository to the real database instead of mock data.

Create the TouristRouteRepository.cs in the Services folder

![6.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/5.How%20to%20use%20Entity%20Framework/6.1.png)



Write the following code in it

```
using MyTourismSite.Database;
using MyTourismSite.Models;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace MyTourismSite.Services
{
    public class TouristRouteRepository : ITouristRouteRepository
    {

        private readonly AddDbContext _context;
        public TouristRouteRepository(AddDbContext context)
        {
            _context = context;//The Connector

        }
        public TouristRoute GetTouristRoute(Guid touristRouteId)
        {
            return _context.TouristRoutes.FirstOrDefault(n => n.Id==touristRouteId);
        }

        public IEnumerable<TouristRoute> GetTouristRoutes()
        {
            return _context.TouristRoutes;
        }
    }
}
```



Finally, don't forget to update the dependency of Interface `ITouristRouteRepository`

![6.2](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/5.How%20to%20use%20Entity%20Framework/6.2.png)

Now you have connected to the real database through EF Core. But currently the database is empty. The next article will solve this problem.