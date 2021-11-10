---
title: Connect to A Mock Repository
tags: .Netcore/API/MVC
author: Anning Mao
---



For .Net Core's MVC architecture, the data model is in the model folder

We need to create a total of 6 models, namely Touristroute, TouristRoutePicture, User, Character, Order and Cart

Take touristroute, TouristRoutePicture as examples here:

```c#
//Models->TouristRoute.cs
public class TouristRoute{
    public Guid Id { get; set; }
    public string Title { get; set; }
    public string Description { get; set; }
    public decimal OriginalPrice { get; set; }
    public decimal? DiscountPresent { get; set; }
    public DateTime CreateTime { get; set; }
    public DateTime UpdateTime { get; set; }
    public DateTime DepartureTime{get;set;}
    public string Feature { get; set; }
    public string Fees { get; set; }
    public string Notes { get; set; }
    public ICollection<TouristPicture> TouristPictures { get; set; } = new List<TouristPicture>();// Foreign key relationship to TouristRoutePicture
}
```

```c#
//Models->TouristPicture.cs
public class TouristPicture
    {
        public int Id { get; set; }
        public string Url { get; set; }
        public Guid TouristRouteId { get; set; }//Foreign key
        public TouristRoute TouristRoute { get; set; }//Foreign key relationship to TouristRoute
    }
```



## Create Repository Interface and a Mock Repository

Create repository under Services folder(create this folder if does not exist). Because for our project, the route repository is a service-level component

then create a Interface called ITouristRouteRepository

```c#
//Services->ITouristRouteRepository.cs
public interface ITouristRouteRepository
    {
        IEnumerable<TouristRoute> GetTouristRoutes();
       TouristRoute GetTouristRoute(Guid touristRouteId);
    }
```

In order to facilitate understanding and testing, instead of linking to the real database, a mock data is used.

```c#
//Services->MockTouristRouteRepository.cs
public class MockTouristRouteRepository : ITouristRouteRepository
    {  
        private List<TouristRoute> _routes;// A list of virtual data instead of a real database
        public MockTouristRouteRepository()
        {
            if (_routes == null)
            {
                InitializeTouristRoutes();
            }
        }
       private void InitializeTouristRoutes()
        {
            _routes = new List<TouristRoute>
            {
                new TouristRoute
                {
                    Id=Guid.NewGuid(),
                    Title="King's park",
                    Description="King's park is so fun",
                    OriginalPrice=1200,
                    Feature="<p>The largest urban park in the southern hemisphere</p>",
                    Fees="<p>FREE</p>",
                    Notes="<p>Beware of danger</p>"
                },
                new TouristRoute
                {
                    Id=Guid.NewGuid(),
                    Title="Fremantle",
                    Description="Fremantle is so fun",
                    OriginalPrice=1100,
                    Feature="<p>a port city</p>",
                    Fees="<p>FREE</p>",
                    Notes="<p>Beware of danger</p>"
                }
            };
        }
        public TouristRoute GetTouristRoute(Guid touristRouteId)
        {//linq
            return _routes.FirstOrDefault(n => n.Id == touristRouteId);
        }

        public IEnumerable<TouristRoute> GetTouristRoutes()
        {
            return _routes;
        }
    }
```



## Register the service dependency of the repository

Add one line in Startup->public void ConfigureServices(IServiceCollection services)

```
services.AddTransient<ITouristRouteRepository,MockTouristRouteRepository>();
```

AddTransient:Each request will create an independent data repository

AddSingleton:Create a unique repository at system startup

AddScoped: Create a unique repository in one event

Here, we just build a framework without any details, so just start with ` AddTransient` 



## Bind controller

```c#
public class TouristRoutesController : ControllerBase
    {

        private ITouristRouteRepository _touristRouteRepository;
        public TouristRoutesController(ITouristRouteRepository touristRouteRepository)//Inject the repository service in the constructor
        {
            _touristRouteRepository = touristRouteRepository;
        }

        // GET: api/<TouristRoutesController>
        [HttpGet]
        public IActionResult GetTouristRoutes()
        {
            var routes = _touristRouteRepository.GetTouristRoutes();
            return Ok(routes);
        }
    }
```



## Test it

then run your project and Enter ` localhost:xxxx/api/TouristRoutes` in the browser address bar 

If browser shows the mock data then it succeeded

![1.1](https://github.com/AnningMao/MarkDownImage/raw/main/.net%20note/1.2%20Mock%20repository/1.1.png)

