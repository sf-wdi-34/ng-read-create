

<h1 id="ng-UD">Angular `$http` Update and Delete</h1>

| Objectives |
| :--- |
| *Students will be able to:* |
| Make requests to update and delete data in the DB using `$http`. |
| Successfully update and delete data in the view once we receive a success message from the DB. |



### Update a database using `$http`

<details>
  <summary>
  **Pseudocode an update function on the client side**
  </summary>
  1. Gather the proper resources to send the request
    1. Specify the proper endpoint on the API to update the proper resource.
    2. Select the proper data to send in this PUT request.
  3. Make the PUT request with all of the proper data.
  4. If a successful response comes back, update the data in your view.
  5. If an error response comes back, log the error, optionally, notify the user of the error, and do not update the data in the view.
</details>
<br>
<details>
  <summary> **Update a resource -- with an example `$http` request to `PUT /api/books/:id`.** </summary>


  ```js
  vm.sendUpdate = function(book){
    $http({
      method: 'PUT',
      url: '/api/books/'+book._id,
      data: {
        title: book.title,
        author: book.author,
        characters: book.characters
      },
    }).then(function successCallback(response) {
      // update the data that's bound to the view.
    }, function errorCallback(error) {
      console.log('There was an error', error);
    })
  };
  ```

  ... and a sample response:
  <details><summary>click to see full response</summary>
  ```js
  {
    "data": {
      _id: "56fd8372m098ok2u89uclwm09",
      title: "Harry Potter and the Sorcerer's Stone",
      author: "J.K. Rowling",
      characters: [ "Harry Potter", "Ron Weasley", "Hermione Granger", "Hagrid", "Dumbledore"]
    },
    "status": 200,
    "config": {
      "method": "PUT",
      "transformRequest": [
        null
      ],
      "transformResponse": [
        null
      ],
      "data": {
        _id: "56fd8372m098ok2u89uclwm09",
        title: "Harry Potter and the Sorcerer's Stone",
        author: "J.K. Rowling",
        characters: [ "Harry Potter", "Ron Weasley", "Hermione Granger", "Hagrid", "Dumbledore"]
      },
      "url": "http://www.cf-books.com/api/books/56fd8372m098ok2u89uclwm09",
      "headers": {
        "Accept": "application/json, text/plain, */*"
      }
    },
    "statusText": "OK"
  }
  ```  
  </details>

</details>


### Delete an entry in the database using `$http`

<details>
  <summary>
  **Pseudocode an update function on the client side**
  </summary>
  1. Gather the proper resources to send the request
    1. Get the proper endpoint on the API to update the proper resource. Make sure you know the way you are supposed to identify a specific item to delete (by id? by name?). For example, `/api/albums/:id`.
  3. Make the DELETE request to the proper endpoint.
  4. If a successful response comes back, update the data in your view.
  5. If an error response comes back, log the error, optionally, notify the user of the error, and do not update the data in the view.
</details>
<br>
<details>
  <summary>**Delete a resource -- with an example `$http` request to `DELETE /api/books/:id`.**</summary>
  ```js
  vm.deleteBook = function(book){
    $http({
      method: 'DELETE',
      url: '/api/books/' + book._id,
    }).then(function successCallback(response) {
      // delete the entry from the data that's bound to the view.
    }, function errorCallback(error) {
      console.log('There was an error', error);
      // Possibly, display to the user that you were unable to delete.
    });
  };
  ```

  ... and a sample response:
  <details><summary>click to see full response</summary>
  ```js
  {
    "data": {
      _id: "56fd8372m098ok2u89uclwm09",
      title: "Harry Potter and the Sorcerer's Stone",
      author: "J.K. Rowling",
      characters: [ "Harry Potter", "Ron Weasley", "Hermione Granger", "Hagrid", "Dumbledore"]
    },
    "status": 200,
    "config": {
      "method": "DELETE",
      "transformRequest": [
        null
      ],
      "transformResponse": [
        null
      ],
      "url": "http://www.cf-books.com/api/books/56fd8372m098ok2u89uclwm0",
      "headers": {
        "Accept": "application/json, text/plain, */*"
      }
    },
    "statusText": "OK"
  }
  ```  
  </details>

</details>

### Exercises

[Sprint 3 of tunely-angular](https://github.com/sf-wdi-31/tunely-angular/blob/master/docs/sprint3.md) - Don't forget to checkout `solutions_sprint_2` and follow the [branching instructions](https://github.com/sf-wdi-31/tunely-angular/blob/master/docs/starting_with_a_branch.md#subsequent-sprints);
