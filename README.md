<!--
location: SF
-->

<img src="https://cloud.githubusercontent.com/assets/7833470/10899314/63829980-8188-11e5-8cdd-4ded5bcb6e36.png" height="60">

| Objectives |
| :--- |
| Make API requests with `$http`. |
| When a form is submitted, send data from its input fields in a request. |
| Send query string parameters with requests. |

<h1 id="ng-CR">Angular `$http` Read and Create</h1>

## Angular `$http`


We **should** know jQuery's `$.ajax` by now.

<details>
  <summary>What does this method do?</summary>

  > It makes HTTP calls asynchronously from our browser and allows us to request information over HTTP without interrupting the front-end or causing page reloads.
</details>

In our projects, we've used this method often, and we've seen the `$.get` and `$.post` shorthand versions.


Angular has a stand-in for `$.ajax` called `$http`.  `$http` is an Angular service.


We can use `$http` from directly within a controller, so we'll try that to get started. When we talk more about services, we'll look at another option.

### Check for Understanding

Look at the `$.ajax()` call below. What does each line do?

```js
$.ajax({
  method: 'GET',
  url: baseUrl + '/api/projects',
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



```js
$http({
  method: 'GET',
  url: baseUrl + '/api/projects'
}).then(function successCallback(response) {
  console.log('response for all projects:', response);
}, function errorCallback(error) {
  console.log('There was an error getting the data', error);
});
```





### Echo App

The popular front-end code sharing website jsfiddle has an "echo" api that responds with information about the requests it gets. This simple [Echo App](https://jsfiddle.net/9L7wae3g/) uses the echo api to show off Angular's `$http`.  You should be able to answer these questions. If not, ask!

* Where is `echoApp` defined?
* What is the name of the `ng-app` used on this page?
* Where is `EchoController` defined?
* What is the name of the `ng-controller` used on the page?
* How did we inject (include) required libraries for a controller?
* How did we make a GET request to an API?
* How did we set the API endpoint URL?
* How did we send query parameters to the API?
* How did we send data to the API?
* Where is the success callback defined?
* Where is the error callback defined?
* How do we set `results` to be the response from an API?

## CRUD operations with `$http`

Here are a few examples of how we'd use `$http` to access an API that describes a man named Jon Snow and his projects. Let's assume the base URL is `http://www.jonsnow-portfolio.com`, and that Jon has his site configured to allow cross-origin requests. (Just like with `$.ajax`, we can leave this part out when we're serving our front-end from the same computer as our back-end.)

### Projects Index

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



### Show one Project

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



### Search Projects

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



### Create Project

To create an instance of a resource, we almost always need to send along some data. The data usually comes from a form.

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
      status: 'ongoing'
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

<details><summary>**In Angular, how do we get form data when it's submitted and serialize it?**  Getting the data is a simple application of `ng-model`, with an object waiting in the controller to track form data. To enable submit, we'll need a submit button inside the form, an `ng-submit` attribute in the form tag, and a function in the controller to handle the submit event. Click for sample code!</summary>


  ```html
  <!-- html -->
  <form ng-submit="projectCtrl.createProject();">
    <input type="text" class="form-control" placeholder="project type" ng-model="projectCtrl.newProject.type"></textarea>
    <!-- other inputs here -->
    <input type="submit">
  </form>
  ```

  ```js
  // inside ProjectController
  vm.createProject = function(){
    console.log('creating project!');
    // make the http request!
    // remember to use the data you have from two-way binding
  }
  ```
</details>


### And so on!

With these examples, you have all the tools you need to figure out how to make requests to update or delete data, too! But we'll take a closer look soon.

## Your Turn

We've gone over how to build forms and work with remote servers using Angular. Now it is your turn to connect an angular front-end to an API.

Tackle [Tunely Sprint 2](https://github.com/sf-wdi-31/tunely-angular/blob/master/docs/sprint2.md)!

If you finish early, try one of these **extra challenge ideas:** Use Angular's `$http` to retreive data from giphy or get and set data in your personal api!
