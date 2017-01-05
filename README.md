<!--
location: SF
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)


# Angular `$http` Create & Read

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

 If you want to integrate social media, live searched images, or most other data into an app, you're probably going to rely on some kind of web API and AJAX. Letting users create, read, update and delete data in a database also requires HTTP requests. Since Angular excels at building single-page apps, we'll take advantage AJAX to avoid refreshing.

Angular has an `$http` service for making AJAX requests.  Its syntax is different from what we saw with jQuery's `$.ajax()` method, so we'll see an Angular way of doing AJAX requests. 

 This topic in particular is also a great example of how time you spend learning one library or framework can carry over to other tools.

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

- Inject a service to use it in an Angular controller.
- Complete a GET request with Angular’s `$http` service.
- Send Angular form data in the body of a POST request, and handle the response.
- Send query string parameters with a request.

### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

- Define AJAX and explain what benefits it has for a site.
- Explain the structure of jQuery's `$.ajax` method.
- Compare and contrast query parameters with request body data.
- List RESTful routes for a JSON data web API.

## Review

1. What is an API?
  <details><summary>click to see an excerpt from previous notes</summary>

  An Application Program Interface (API) is the way in which you interact with a piece of software. In other words, it is the interface for an application or a program.

    * Many organizations have web APIs allowing people to send them queries and receive data (e.g. [GitHub API](https://developer.github.com/v3) ), but this is just one type of API.
    * Remember, even an `Array` has an API -- all the methods that can be called on it, such as: `.forEach`, `.pop`, `.length` etc.
  </details>

1. What does jQuery's `$.ajax` method do?

  <details><summary>click to see an excerpt from previous notes</summary>

  > It makes HTTP calls asynchronously from our browser and allows us to request information over HTTP without interrupting the front-end or causing page reloads.

  </details>


### Check for Understanding

Look at the `$.ajax()` call below. Walk through it line by line.

```js
$.ajax({
  method: 'GET',
  url: 'http://www.jonsnow-portfolio.com/api/projects',
  success: onSuccess,
  error: onError
});
function onSuccess(response) {
  console.log('response for all projects:', response);
}
function onError(xhr, status, error) {
  console.log('There was an error getting the data', error);
}
```


Next, check out this AJAX request with Angular's `$http` service.  What do you think is happening on each line?


```js
$http({
  method: 'GET',
  url: 'http://www.jonsnow-portfolio.com/api/projects'
}).then(successCallback, errorCallback);

function successCallback(response) {
  console.log('response for all projects:', response);
}
function errorCallback(error) {
  console.log('There was an error getting the data', error);
}
```

1. What similarities do you see between the code blocks above?

  <details><summary>click to list a few:</summary>  
    *  method, url  
    *  callbacks for success and error  
  </details>

  > Why do you think both jQuery and Angular have AJAX methods that share the features you noticed?

1. What are some major differences?

  <details><summary>click to list a few:</summary>  
    * `.then`    
    *  parameters of the error callback    
  </details>


### Service Setup

Not all of Angular is loaded into every module and controller. Instead, you have to include or "inject" dependencies into parts of your app where you want to use them. This helps with minification, but it also makes it easier to configure, reuse, and test componentss.

The [`$http`](https://docs.angularjs.org/api/ng/service/$http) service is a core part of Angular. Still, to use it in our controller, we first need to tell Angular that we'd like to have it available by `inject`ing it. To do this, we include one extra line with the controller function definition. Here's the syntax:

  ```javascript
  // app.js
  GifIndexController.$inject = ['$http'];
  function GifIndexController (  $http  ) {
    ...
  }
  ```


### Independent Practice: Echo App

The popular front-end code sharing website jsFiddle has an "echo" API that responds with information about the requests it gets. This simple [Echo App](https://jsfiddle.net/xh1bo85v/2/) uses the echo API to show off Angular's `$http` service.  You should be able to answer these questions.

1.  We don't see the entire HTML file in a jsFiddle, but `echoApp` is the name of the `ng-app` used on this page. Where is `echoApp`  defined in the code we can see?

2.  What is the name of the `ng-controller` used on the page?

3.  Where is `EchoController` defined?

4.  Where is `echoCtrl` defined?  How is it different from `EchoController`?

5.  How did we inject (include) the `$http` service into the controller?

6.  How did we configure the `$http` call's request types for GET and POST?

7.  How did we set the API endpoint URL?

8.  Which request used query parameters? How did we send query parameters to the API?

9.  Which request had data in the body? How did we send request body data to the API?

10.  Where are the success callbacks defined?

11.  Where is the error callback defined?

12.  What code is displaying the API's responses on the page?


### CRUD operations with `$http`

What follows are examples of how we'd use `$http` to access an API that describes a man named Jon Snow and his projects. Let's assume the base URL is `http://www.jonsnow-portfolio.com`, and that Jon has his site configured to allow cross-origin requests. (Just like with `$.ajax`, we can leave this part out when we're serving our front-end from the same computer as our back-end.)

### Read

#### Projects Index

Remember, we use index to mean getting all of a resource! And remember your RESTful routes.

<details>
  <summary>**Get all projects -- with an `$http` request to `GET /api/projects`.**</summary>
  ```js
  $http({
    method: 'GET',
    url: baseUrl + '/api/projects'
  }).then(function successCallback(response) {
    console.log('response for all projects:', response);
    // probably do something with the response data
  }, function errorCallback(error) {
    console.log('There was an error getting the data', error);
  });
  ```

  ... and a sample response:
  <details><summary>click to see full response</summary>
  ```js
  {
    "data": [
       {
          _id: 2,
          name: 'Defeat the wildlings',
          type: 'quest',
          opponents: [ 'Mance Rayder', 'Lord of Bones'],
          status: 'resolved'
       },
       {
          _id: 3,
          name: 'Save the wildlings',
          type: 'campaign',
          opponents: ['the Night Watch', 'the Others'],
          status: 'pending'
       }
    ],
    "status": 200,
    "config": {
      "method": "GET",
      "transformRequest": [
        null
      ],
      "transformResponse": [
        null
      ],
      "url": "http://www.jonsnow-portfolio.com/api/projects",
      "headers": {
        "Accept": "application/json, text/plain, */*"
      }
    },
    "statusText": "OK"
  }
  ```  
  </details>

</details>



#### Show One Project

Showing just one instance of a resource often requires an id.  With `$http`, we'll just add that to the `url`.

<details>
  <summary>**Show a project -- with an example `$http` request to `GET /api/projects/3`.**</summary>
  ```js
  $http({
    method: 'GET',
    url: baseUrl + '/api/projects/3',
  }).then(function successCallback(response) {
    console.log('response for show project 3:', response);
  }, function errorCallback(error) {
    console.log('There was an error', error);
  });
  ```

  ... and a sample response:
  <details><summary>click to see full response</summary>
  ```js
  {
    "data": {
      _id: 3,
      name: 'Save the wildlings',
      type: 'campaign',
      opponents: ['the Night Watch', 'the Others'],
      status: 'pending'
       },
    "status": 200,
    "config": {
      "method": "GET",
      "transformRequest": [
        null
      ],
      "transformResponse": [
        null
      ],
      "url": "http://www.jonsnow-portfolio.com/api/projects/4",
      "headers": {
        "Accept": "application/json, text/plain, */*"
      }
    },
    "statusText": "OK"
  }
  ```  
  </details>

</details>



#### Search Projects

Searching often uses query string parameters, which we can send through `$http` with an option called `params`.

<details>
  <summary>**Search projects -- with an `$http` request to `GET /api/projects/search?type=quest`.**</summary>
  ```js
  $http({
    method: 'GET',
    url: baseUrl + '/api/projects',
    params: {
      type: "quest"
    },
  }).then(function successCallback(response) {
    console.log('response for "quest" project search:', response);
  }, function errorCallback(error) {
    console.log('There was an error getting the data', error);
  });
  ```

  ... and a sample response:
  <details><summary>click to see full response</summary>
  ```js
  {
    "data": [
       {
          _id: 2,
          name: 'Defeat the wildlings',
          type: 'quest',
          opponents: [ 'Mance Rayder', 'Lord of Bones'],
          status: 'resolved'
       }
    ],
    "status": 200,
    "config": {
      "method": "GET",
      "transformRequest": [
        null
      ],
      "transformResponse": [
        null
      ],
      "params": {
        "type": "quest"
      },
      "url": "http://www.jonsnow-portfolio.com/api/projects/search",
      "headers": {
        "Accept": "application/json, text/plain, */*"
      }
    },
    "statusText": "OK"
  }
  ```  
  </details>

</details>




### Create

To create an instance of a resource, we almost always need to send along some data. The data is usually sent in the request body.

<details>
  <summary>**Create project -- with an example `$http` request to `POST /api/projects`.**</summary>
  ```js
  $http({
    method: 'POST',
    url: baseUrl + '/api/projects',
    data: {
      name: 'Mentor new members of the Night\'s Watch',
      type: 'volunteering',
      opponents: [ ],
      status: 'resolved'
    },
  }).then(function successCallback(response) {
    console.log('response for create project:', response);
  }, function errorCallback(error) {
    console.log('There was an error getting the data', error);
  });
  ```

  ... and a sample response:
  <details><summary>click to see full response</summary>
  ```js
  {
    "data": {
      _id: 4,
      name: "Mentor new members of the Night's Watch",
      type: "volunteering",
      opponents: [ ],
      status: "ongoing"
    },
    "status": 200,
    "config": {
      "method": "POST",
      "transformRequest": [
        null
      ],
      "transformResponse": [
        null
      ],
      "data": {
        name: "Mentor new members of the Night's Watch",
        type: "volunteering",
        opponents: [ ],
        status: "ongoing"
      },
      "url": "http://www.jonsnow-portfolio.com/api/projects",
      "headers": {
        "Accept": "application/json, text/plain, */*"
      }
    },
    "statusText": "OK"
  }
  ```  
  </details>

</details>


### Working with Forms

Searching isn't as simple as just making an AJAX request, though. We usually let the user enter a search term into a form. The same goes for creating something: we usually let a user enter in information to be POSTed.

1. In Angular, how do we dictate what should happen when a form is submitted?  
  * How would we do this with jQuery?  
  * Have you seen any other event listeners in Angular that you could compare to?

  > <details><summary>click for how to</summary>
      To enable submit, we'll need a submit button inside the form, an `ng-submit` attribute in the form tag, and a function in the controller to handle the submit event.
      </details>

1. In Angular, how can we get the value of a form's input fields?
  * How can we associate part of the view with a value from the controller?

  > <details><summary>click for how to</summary>
  This sounds like a job for `ng-model`!  When a form has multiple inputs, it's common to group those into one larger object in the controller.
  </details>



<details><summary>Click to see this form setup in sample code.</summary>


    ```html
    <!-- html -->
    <form ng-submit="projectCtrl.createProject();">
      <input type="text" class="form-control" placeholder="project name" ng-model="projectCtrl.newProject.name"></input>
      <input type="text" class="form-control" placeholder="project type" ng-model="projectCtrl.newProject.type"></input>
      <!-- other inputs here -->
      <input type="submit">
    </form>
    ```

    ```js
    // inside ProjectController
    vm.createProject = function(){
      console.log('creating project!');
      // make the http request with the data you have from two-way binding
    }
    ```
</details>

### Closing Thoughts

* As we move into new technologies, you should start to notice your prior experience paying off.
* The `$http` service is just one of many services you might want to use in an Angular app.  
* With these examples, you have all the tools you need to figure out how to make requests to update or delete data, too! But we'll take a closer at those soon.
