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
They consist of one or more controller classes that derive from [ControllerBase].

e.g.

![image](https://github.com/user-attachments/assets/93a09cf1-3a7d-4348-a9b4-9ff7b96142be)

The [ControllerBase] class provides many properties and methods that are useful for handling HTTP requests. 

e.g. 
The following code sample [CreatedAtAction] returns a 201 status code:

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

