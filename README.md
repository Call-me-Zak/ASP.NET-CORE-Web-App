# Deploying an ASP.NET-CORE API to Azure App Service

![image](https://github.com/user-attachments/assets/5efe11f0-a52e-47e4-8d86-fe052006e770)

### Learning Path
In this learning journey I tackled the benefits of using APIs (a set of rules and protocols allowing apps to communicate with each other).

APIs not only provide a way for developers to access the functionality of another piece of software and use it in their own applications, such as a web service.

They also : 
- Simplify development ==> functionality of other software is accessible without having to understand the details of how that software works, saving time and effort when building new apps.
- Enable integration ==> allow different software apps to work together,  helping businesses integrate their systems and data and improving efficiency and productivity.

### Objectives
- Describe the two model types of APIs in ASP.NET Core.
- Create Swagger documentation for an API by using Swashbuckle.
- Interact with an API by using Swagger interface.

### Skills Learned
- Navigated a documented API
- Determined endpoints for HTTP operations e.g. /swagger
- Identified API requirements for HTTP operations
- Deployed the API to Azure App Service

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
    app.UseSwagger();            // Adds Swagger middleware to serve the Swagger UI and JSON endpoint
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

##### Note : 
The default endpoint for the Swagger UI is ```https:localhost:port/swagger```, in our case we used ```port 5001``` for ```HTTPS``` (use ```port 5000``` for ```HTTP``` when running locally.)

But this isn't a rule of thumb, as we can also change to any port we want by modifying the ```launchSettings.json``` file or by setting the ```ASPNETCORE_URLS``` environment variable (check the ````launch.json``` file under ```env```).

![image](https://github.com/user-attachments/assets/d06c0294-47a3-4fa1-a74d-e1236341769e)


These are merely default setting and we can configure the app to run on a different port if needed.
In the ```launchSettings.json``` file, change the ```applicationUrl``` port number to the one we want and then SwaggerUI will be accessible on that port instead.

![image](https://github.com/user-attachments/assets/45013dec-f86c-4e45-966f-ccddf6b6702b)

Next, we proceed to add descriptive headings to the different functions in our API by using the ```.WithTags``` option. The following sample code shows adding "Add fruit to list" as a heading to a POST mapping:

![image](https://github.com/user-attachments/assets/84a1e312-f500-4c41-a30c-d5233cc2444b)


The Swagger UI displays the "Add fruit to list" tag as a heading above the POST action.

![image](https://github.com/user-attachments/assets/778e77ec-221a-41b6-acc6-35bd4cfbe4fe)


Ref : SwaggerUI accessed locally via ```https://localhost:5050/swagger/v1/swagger.json```
I will now walk you through the code used to get to that result.

![image](https://github.com/user-attachments/assets/95026cc3-c2f4-46dd-8870-bb7bca535269)

### Skills Learned :

Navigate a documented API
Determine endpoints for HTTP operations
Identify API requierments for HTTP operations
Publish an app to Azure App Service

#### Step 01 :
###### API information
The API interacts with an in-memory database that contains the following fields:
![image](https://github.com/user-attachments/assets/9282ffea-e769-441a-ac28-9b6eebe723fc)

The Swagger documentaiton was created by using the Swashbuckle package.

#### Step 02 :
##### Running the Fruit API locally (Fruit API download link: https://raw.githubusercontent.com/MicrosoftLearning/APL-2002-develop-aspnet-core-consumes-api/master/Allfiles/Downloads/FruitAPI.zip)

1) Launch VSCode > File > Open Folder

![image](https://github.com/user-attachments/assets/086bc8e2-8773-4fbf-a86e-56763a92e889)

2) Select the ````FruitAPI``` folder

![image](https://github.com/user-attachments/assets/24dc8bf3-d43d-4376-8d7f-fbe0fa75bcee)

3) The files should appear in the Explorer pane

![image](https://github.com/user-attachments/assets/d96f86e5-71ef-4491-af1d-f53df2a0287e)

4) From the cmd prompt, execute the ```dotnet run``` command to start our work.
   Note that we need to navigate to the directory where the project is located for the command to work, and for this particular project we'll need .NET 7.0.20, as I had .NET 8.0.7, I needed to download the proper version as indicated in the picture below.

![image](https://github.com/user-attachments/assets/7d245c0e-c497-4ad2-bc47-0f736285c74f)

![image](https://github.com/user-attachments/assets/aa0155b7-e76b-47d9-8e92-c6e940e75080)
![image](https://github.com/user-attachments/assets/09208fb6-e033-464e-9548-cd5f6377db40)

Our project is now using .NET 7.0. as can be seen our dependencies file :

![image](https://github.com/user-attachments/assets/843a59ce-37ed-42db-8337-4db7ffdcb25b)

And TADA ! Works like a charm
###### Note :
```http://localhost/5050``` will display this page cannot be found message, unless we append ```/swagger``` as that is the endpoint for Swagger API documentation.

 Full URL : ```http://localhost/5050/swagger```

![image](https://github.com/user-attachments/assets/a404edda-127f-41c1-9c00-5ddf47168077)

![image](https://github.com/user-attachments/assets/83d88af2-a7f2-4703-9b2b-bf3127743f88)


#### Step 03 : Performing different operations on the data :

##### 1) ````GET``` operation :
By clicking ```GET``` we get the following menu :

![image](https://github.com/user-attachments/assets/e30e1fdd-30b7-4070-b1db-d3eef0b93fa6)

After clicking on ````Try it out``` then ```Execute``` we're then presented with a list of fruits and their availability in a .json format

![image](https://github.com/user-attachments/assets/e62b8af0-31df-4eb8-8863-243e0ec4f69e)

![image](https://github.com/user-attachments/assets/33066749-7053-4e0d-9fda-d3219e677fa8)

And if we append ```/fruitlist``` to our url instead of ```/swagger```we can directly access the page containing the JSON package :

![image](https://github.com/user-attachments/assets/10871675-7a96-4cb0-b6fc-70f07c8e35ba)

##### 2) ```POST``` operation :
We can see here that a request body is required, which makes sense as this is a POST request.

![image](https://github.com/user-attachments/assets/eca59fb0-27d1-41cf-b696-bdabb8b0ba22)

Response :

![image](https://github.com/user-attachments/assets/1a22fd13-32aa-4495-94e8-f0d95bd77c64)

This time, to directly access the page, we append ```/5``` to the URL.

![image](https://github.com/user-attachments/assets/8692cdb3-ce7f-46d3-8232-54b4d149ccbc)

Now, if we try and append a different number before using the post request we'll get the following error :

![image](https://github.com/user-attachments/assets/15ced153-b36e-4c7c-8717-05f88e84dfb2)

But after the ```POST``` request we can see that we get a succesful response.

![image](https://github.com/user-attachments/assets/b25fbe82-04f8-4b98-abed-a90979c865f0)

![image](https://github.com/user-attachments/assets/12bd6eaa-4522-44ac-b337-c19dae5caaf9)

So, I went ahead and added a "Pear" to our fruit basket.
I left the id 0 so an index will be automatically assigned (because ```id : 0``` was taken) , as we can see in the 2nd picture, it's 15.

![image](https://github.com/user-attachments/assets/691aa1b8-1cbb-4e77-ae71-5491a1097579)

![image](https://github.com/user-attachments/assets/9e390d8b-b0b2-4962-8d91-62bf5ccc8b2f)

So, if I were to go back and use ```GET``` request, we'll see new elements have now been added.

![image](https://github.com/user-attachments/assets/043dac6a-d80e-45f8-affa-690fc86a81ac)

We can find the Pears that I added with the ```POST``` request in there as well.

![image](https://github.com/user-attachments/assets/7066ec00-b789-4ae7-bada-cbf2cb316c64)


##### 3) ```DELETE``` operation :
We can delete by id, so I'm gonna go ahead and delete ids 4 to 13, as they all contain the same element.

![image](https://github.com/user-attachments/assets/a085263b-cb5a-4bd9-a76e-c1357ab6c096)

![image](https://github.com/user-attachments/assets/a1c13d46-30c8-4e35-ae5b-1677373bc905)

Under ```Responses``` we can see that the operation was a success.

![image](https://github.com/user-attachments/assets/beae4835-69e2-4ba4-9dbd-0aac84589a5d)

To verify, we simply use ```GET``` request to check.
And the JSON element with ```id: 4``` has been removed.

![image](https://github.com/user-attachments/assets/baee64d1-7cf5-4520-bbf8-d55036aa00f5)

![image](https://github.com/user-attachments/assets/add8fa94-9b16-4724-a0df-a143f02fb052)

And if I were to try and delete it again, we get the infamous error 404 not found:

![image](https://github.com/user-attachments/assets/cdaa425f-8e08-4a16-a42f-ba6a3f5d190b)

And after deleting ids from 4 to 13, we can check with ```GET``` to see the remaining elements.

![image](https://github.com/user-attachments/assets/84874907-083b-43ff-87c6-9bc3170ff2c5)

I also took the liberty of deleting the 2 Pears, leaving only one.

![image](https://github.com/user-attachments/assets/7f480ef7-2208-456b-b7c7-f08b6cdc98c7)

Final result :

![image](https://github.com/user-attachments/assets/07590f9f-c464-431d-816d-40179a42047c)

![image](https://github.com/user-attachments/assets/62762c49-c449-478e-b6a1-c701c2c927de)

And here we can see the instance where I tried to add the same id again vs when a new element has been added

![image](https://github.com/user-attachments/assets/db88e80c-3f72-4c46-a821-6ccfaef21694)

#### Step 04 : Publishing the API to Azure App Service using Azure extensions on VSCode
<a href="https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureresourcegroups">Azure Resources extension</a>

<a href="https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice">Azure App Service extension</a>

##### 1) Sign-in to Azure

![image](https://github.com/user-attachments/assets/c812faa8-c98e-41c0-87ba-765819083de9)

When I first tried to deploy I got an error cause I was on the wrong directory
![image](https://github.com/user-attachments/assets/680a46af-d40a-4c34-bcde-ac003cfaf627)

Let's go ahead and switch to the correct directory

![image](https://github.com/user-attachments/assets/0931c9d1-98aa-4d88-b62f-fa3c309d4611)

I can now select my Azure subscription

![image](https://github.com/user-attachments/assets/2b75392b-842b-41f8-aaea-2a136864541f)

1 : ```CTRL``` + ```SHIFT``` + ```P``` to open up VSCode command menu to filter for the web app in this case

![image](https://github.com/user-attachments/assets/b2e69b66-e7fe-496d-86f2-46738e8fe792)

Alternatively right clicking on ```App Services``` then selecting ```Create New Web App (Advanced)``` does the trick.
![image](https://github.com/user-attachments/assets/419a3002-431c-4dba-b7e4-56f9aadb3eb6)


2
![image](https://github.com/user-attachments/assets/af3c543a-4b2f-4e9e-b45f-da9ea76b2674)

3 : Select ```+Create new resource group``` or pick one if you already have.
![image](https://github.com/user-attachments/assets/a5e903f9-4cb7-4b0f-a11d-2ff234dc6e8f)

4 Select the runtime stack .NET 7 (STS)

![image](https://github.com/user-attachments/assets/42e2cf17-38f6-4757-b76a-b568364cb4c7)

5
![image](https://github.com/user-attachments/assets/13638499-9a3d-475f-8f18-a445c44fea04)

6
![image](https://github.com/user-attachments/assets/47347aab-0724-4b57-9e9b-137369f80bf0)

7
![image](https://github.com/user-attachments/assets/4463faa5-11b5-448d-96b5-5fbaf6a0a137)

8
![image](https://github.com/user-attachments/assets/97a281ae-414c-497e-ae82-6066a1bb0628)

9 Skip For now

![image](https://github.com/user-attachments/assets/5867b6f9-e57f-445a-b784-f358276fd7f8)

This time our web app has been succsefully created, and we get the ```deploy``` button at the bottom right corner
![image](https://github.com/user-attachments/assets/54e32367-8c42-464b-b22a-32c3b37137d3)

We can see our app in the resources tab

![image](https://github.com/user-attachments/assets/60073231-e6b8-41c6-b3e5-8c11fcc306f4)

Since I've yet to deploy it, our lovely API is yet to appear !

![image](https://github.com/user-attachments/assets/85af587a-84f3-4980-964f-2acd2d7f24c0)

We can see here adding ```/swagger``` gives us the error code 404 not found.

![image](https://github.com/user-attachments/assets/d1bca039-0a72-4d84-9ff9-af3a6908ac31)

After deploying

![image](https://github.com/user-attachments/assets/9523bc52-09c8-44fe-8db1-d231d5a0b390)

![image](https://github.com/user-attachments/assets/0ca4d0c7-14a6-4d4c-90d2-19c547675bc4)

![image](https://github.com/user-attachments/assets/ceee0a24-6c6f-48b5-90dd-4ac7e105d1c5)

![image](https://github.com/user-attachments/assets/0980ca91-936e-4fe8-9157-3ce77922f8bf)

Our magnificent API has been succesfully deployed to Azure ! 🥳

![image](https://github.com/user-attachments/assets/fddae279-1ebe-4a0b-b2ab-087b23a20e44)

If you stuck around this long, it also adapts to mobile

![image](https://github.com/user-attachments/assets/1c9c0f3a-d747-4022-bd83-97c7616985cd)

##### Fun fact: 
By the time you're reading this our hardworking API is most likely down, cause Azure requires a subscription fee to keep it up, and you know the limitation that comes with free subscriptions.

A memorial to my lovely API project.
```https://fruitapi-zak.azurewebsites.net/swagger/index.html```


