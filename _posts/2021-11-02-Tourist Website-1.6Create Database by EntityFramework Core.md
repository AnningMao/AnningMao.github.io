---
title: Create Database by EntityFramework Core
tags: .Netcore/API/MVC
author: Anning Mao
---

## Add database restrictions

Validate the data model: add database restrictions, primary key information and foreign key connections, etc.

First add the following references in your model

```
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
```

Then update your code of Models.TouristRoute and Models.TouristRoute as follow:

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace MyTourismSite.Models
{
    public class TouristRoute
    {
        [Key]
        public Guid Id { get; set; }
        [Required]
        [MaxLength(100)]
        public string Title { get; set; }
        [Required]
        [MaxLength(1500)]
        public string Description { get; set; }
        [Column(TypeName = "decimal(18,2)")]
        public decimal OriginalPrice { get; set; }
        [Required]
        [Range(0.0, 1.0)]
        public decimal? DiscountPresent { get; set; }
       
        public DateTime CreateTime { get; set; }
        public DateTime UpdateTime { get; set; }
        public DateTime DepartureTime{get;set;}
        [MaxLength]
        public string Feature { get; set; }
        [MaxLength]
        public string Fees { get; set; }
        [MaxLength]
        public string Notes { get; set; }
        public ICollection<TouristPicture> TouristPictures { get; set; } = new List<TouristPicture>();
    }
}

```

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using System.Linq;
using System.Threading.Tasks;

namespace MyTourismSite.Models
{
    public class TouristPicture
    {
        [Key]
        [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public int Id { get; set; }
        [MaxLength(100)]
        public string Url { get; set; }
        [ForeignKey("TouristRouteId")]
        public Guid TouristRouteId { get; set; }//EF will automatically set the primary key of each model to the form of class name + ID when mapping the database, so [ForeignKey("TouristRouteId")] contains TouristRouteId instead of Id
        public TouristRoute TouristRoute { get; set; }
    }
}
```



## Create database by EF Core Tools

Firstly, you need to Install Entity Framework Core Tools by NuGet

It supports three ways to create a database: database-first, code-first, and model-first.

1. This way will use Package Manage Console(PMC) of Visual studio(View->Other windows->Package Manage Console)

   

   - Create option

     ```bash
     add-migration initialMigration
     ```

     It will create a new folder, Migration, in your Project. The xxxx.cs inside Migration will help you create and delete database by function `Up` and `Down`

   - Update option

     ```
     update-database
     ```

   ![2.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/1.6%20Creat%20Database%20by%20EntityFramework/2.1.png)

2. If you don't like this method or use a mac computer, you can also use the command line to do so.

   After version 3.0, the ef command line tool has been removed from the dotnet SDK, so additional installation is required

   ```bash
   dotnet tool install --global dotnet-ef
   ```

   Create & Update options

   ```bash
   dotnet ef migrations add initialMigration
   dotnet ef database update
   ```


If all goes well, you can see your Database in sql Server Object Explorer

![2.2](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/1.6%20Creat%20Database%20by%20EntityFramework/2.2.png)





 

   







