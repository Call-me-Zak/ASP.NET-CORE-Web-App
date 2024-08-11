# ASP.NET-CORE-Web-App
#### Learning Path
In this learning journey I tackled the benefits of using APIs (a set of rules and protocols allowing apps to communicate with each other).

APIs not only provide a way for developers to access the functionality of another piece of software, such as a web service, and use that functionality in their own applications.
They also : 
- Simplify development ==> functionality of other software is accessible without having to understand the details of how that software works, saving time and effort when building new applications.
- Enable integration ==> allow different software applications to work together,  helping businesses integrate their systems and data and improving efficiency and productivity.

##### Objectives
- Describe the two model types of APIs in ASP.NET Core.
- Create Swagger documentation for an API by using Swashbuckle.
- Interact with an API by using the Swagger interface.

##### Approach
To simplify the learning process, the work was segmented into digestable units.

### Unit 01 : ASP.NET Core APIs
ASP.NET Core supports two approaches to creating APIs: a controller-based approach and minimal APIs. A controller-based API is a traditional approach to building APIs in which each endpoint is mapped to a specific controller class. The controller handles the request, performs any necessary business logic, and returns a response.

##### Controller-based API
They consist of one or more controller classes that derive from `ControllerBase`.

e.g.

![image](https://github.com/user-attachments/assets/93a09cf1-3a7d-4348-a9b4-9ff7b96142be)

The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests. 

e.g. 
The following code sample `CreatedAtAction` returns a 201 status code:

![image](https://github.com/user-attachments/assets/c822a1ce-2f5a-42d6-b184-5b38b90afd6d)

##### Minimal API
Build fully functioning REST endpoints with minimal code and configuration. Skip traditional scaffolding by declaring API routes and actions.
Minimal APIs support the configuration and customization needed to scale to multiple APIs, handle complex routes, apply authorization rules, and control the content of API responses.

e.g.
The following code creates an API at the root of the web app that returns the text, "Hello World!":

![image](https://github.com/user-attachments/assets/2c8f031a-2d52-4313-881b-2a5a3edb3b94)

Most APIs accept parameters as part of the route.

![image](https://github.com/user-attachments/assets/79c27285-fb8f-46a5-9acd-db5deba40c06)


### Unit 02 : Document an API by using Swashbuckle
Swashbuckle helps automatically generate comprehensive, interactive RESTful API documentation.

There're 03 main components to Swashbuckle :
##### Swashbuckle.AspNetCore.Swagger
Tool used for writing down API details in Swagger JSON format.

Package installation :
```dotnet add package Swashbuckle.AspNetCore.Swagger --version 6.7.0```


##### Swashbuckle.AspNetCore.SwaggerGen
Tool used for generating documentation for API code.

Package installation :

```dotnet add package Swashbuckle.AspNetCore.SwaggerGen --version 6.7.0```

##### Swashbuckle.AspNetCore.SwaggerUI
UI for interacting with the API documentation.

Package installation :

```dotnet add package Swashbuckle.AspNetCore.SwaggerUI --version 6.7.0```

Note that these 03 packages can all be installed using one command:

```dot add package Swashbuckle.AspNetCore -v -6.5.0```

It's also worth adding (no pun intended) that all the commands will install the package to the ```.csproj``` file in the current directory and does not install globally, so if you have multiple ```.csproj``` files in the same directory, you'll need to specify the name of your ```.csproj``` project by adding ```<project_name>.csproj``` so the command you'll be using in that case would be :

```dotnet add package <project_name>.csproj Swashbuckle.AspNetCore -v -6.5.0```

After installing the package, we need to add and configure Swagger middleware:
```
var builder = WebApplication.CreateBuilder(args);

// Register services for the application
builder.Services.AddEndpointsApiExplorer();  // Enables endpoint discovery
builder.Services.AddSwaggerGen();            // Adds Swagger generation services

var app = builder.Build();

// Configure the HTTP request pipeline
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();            // Adds Swagger middleware to serve the Swagger UI and JSON endpoint (The default endpoint for the Swagger UI is http:<hostname>:<port>/swagger)
    app.UseSwaggerUI();          // Configures Swagger UI to visualize and interact with the API
}

app.UseAuthorization();

app.MapControllers();

app.Run();

```

We can also customize and extend the Swagger documentation :
The configuration action passed to the ```AddSwaggerGen``` method can include additional information through the ```OpenApiInfo``` class.

We can add information to display in the API documentation :

![image](https://github.com/user-attachments/assets/80dd7b32-b85a-44a2-9a8f-bc7a0b705430)

Result we get from SwaggerUI :

![image](https://github.com/user-attachments/assets/b2391bbe-e211-4749-a295-acf770a05ac1)
