# Angularjs for no-routing SPA

## Angularjs

AngularJS is a popular JavaScript-based open-source front-end web application framework. It was developed by Google and released in 2010. AngularJS is designed to simplify the development of dynamic, single-page applications by providing a structured framework for building reusable components and handling data binding.

Here are some key features and concepts of AngularJS:

1. Two-way data binding: AngularJS allows automatic synchronization of data between the model (JavaScript variables) and the view (HTML elements). When data in the model changes, the corresponding view elements are automatically updated, and vice versa.

2. Directives: Directives are markers in the HTML that tell AngularJS to attach specified behaviors or transformations to a DOM element or its children. AngularJS comes with a set of built-in directives, such as ng-model, ng-repeat, ng-show, etc., and also allows you to create custom directives.

3. Controllers: Controllers are JavaScript functions that are responsible for handling the business logic and data manipulation in a particular part of the application. They interact with the model and provide data and behavior to the view through scope objects.

4. Services: Services are singleton objects that provide common functionality and can be injected into controllers, directives, and other services. They are used for tasks like making HTTP requests, handling authentication, or sharing data between different components.

5. Modules: AngularJS organizes the application into modular units called modules. A module encapsulates related controllers, services, directives, and other components. It helps in managing dependencies and keeping the codebase modular and maintainable.

6. Routing: AngularJS includes a powerful routing mechanism that allows you to define routes and map them to different views and controllers. This enables the development of single-page applications where different views are loaded dynamically without full page reloads.

7. Testing: AngularJS provides built-in support for unit testing and encourages writing testable code. It includes tools like Jasmine and Karma for testing controllers, services, and other components.

It's important to note that AngularJS is an older version of Angular (commonly referred to as AngularJS 1.x), and it has been succeeded by Angular (also known as Angular 2+). Angular, released in 2016, is a complete rewrite of AngularJS and introduces significant changes and improvements in terms of performance, architecture, and tooling. However, AngularJS still has a large user base and many existing applications are built using it.

AngularJS (also known as AngularJS 1.x) is officially discontinued and no longer actively maintained by Google. The last major release of AngularJS was version 1.7.9, which was released in September 2018. Since then, Google has shifted its focus to the newer version of Angular (Angular 2+), which is a complete rewrite of AngularJS.

Google's official announcement stated that AngularJS would enter a long-term support phase, which means that critical bugs and security issues will still be addressed until June 30, 2021. However, there will be no new features or major updates for AngularJS.

While AngularJS is no longer receiving active development and updates from Google, it still has a significant user base and many existing applications built with it. There are also numerous resources and community support available for developers working with AngularJS. However, for new projects, it is generally recommended to use the latest version of Angular, as it offers improved performance, modern features, and ongoing support from Google.

## SPAs

SPA stands for Single-Page Application. It is a web application architecture that allows for a fluid and seamless user experience by loading and manipulating content on a single web page dynamically, without the need for complete page reloads.

In a traditional web application, when a user interacts with the application by clicking on a link or submitting a form, the browser sends a request to the server, and the server responds by generating and sending back a complete HTML page. This process typically involves reloading the entire page, including its layout, stylesheets, and scripts.

In contrast, a Single-Page Application works differently. When a user initially loads the SPA, the browser retrieves a single HTML page that acts as a container for the application. The initial load includes the necessary JavaScript, CSS, and other assets. Once loaded, the SPA communicates with a server through APIs (Application Programming Interfaces) to fetch and update data, rather than reloading the entire page.

## Hello world

```html
<!DOCTYPE html>
<html ng-app="myApp">

<head>
  <meta charset="UTF-8">
  <title>AngularJS SPA</title>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
  <script>
    // Define your AngularJS application module
    var app = angular.module('myApp', []);
    
    // Define your controller
    app.controller('myController', function($scope) {
      // Your controller logic goes here
      $scope.message = "Hello world";
    });
  </script>
</head>

<body ng-controller="myController">
  <h1>{{ message }}</h1>
  <!-- Your HTML content goes here -->
</body>

</html>
```

In this template, we include the AngularJS library from a CDN (Content Delivery Network) using the `<script>` tag. You can replace the URL with the specific version you want to use.

Inside the `<script>` tag, we define an AngularJS application module named "myApp" using the `angular.module` function. We also define a controller named "myController" using the `app.controller` function. You can add your own logic and functionality inside the controller.

In the HTML body, we use the `ng-app` directive to specify that the "myApp" module should be used as the application's root module. The `ng-controller` directive is used to associate the "myController" with a particular element, in this case, the `<body>` tag.

Within the `<h1>` tag, we use the AngularJS expression `{{ message }}` to display the value of the `$scope.message` variable, which is defined in the controller.

Feel free to modify and expand upon this template according to your specific requirements and application logic.

## Directives

1. ng-model: This directive is used for two-way data binding between the view (HTML) and the model (JavaScript). It binds the value of an input element to a variable in the controller.

2. ng-show / ng-hide: These directives show or hide elements based on a condition. The element is shown if the condition evaluates to true, and hidden if it evaluates to false.

3. ng-if: This directive conditionally renders or removes an element from the DOM based on a condition. If the condition is true, the element is added to the DOM; otherwise, it is removed.

4. ng-repeat: This directive is used to repeat a set of HTML elements for each item in an array or object. It creates a clone of the HTML template for each item in the collection.

5. ng-click: This directive binds a click event to a function in the controller. When the element is clicked, the associated function is executed.

6. ng-class: This directive dynamically applies CSS classes to an element based on a condition. It allows you to add or remove classes based on the state of your application.

Here's an example that demonstrates the usage of these directives in a Single-Page Application using AngularJS:

```html
<!DOCTYPE html>
<html ng-app="myApp">

<head>
  <meta charset="UTF-8">
  <title>AngularJS Directives Example</title>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
  <script>
    var app = angular.module('myApp', []);

    app.controller('myController', function($scope) {
      $scope.showContent = false;
      $scope.message = '';
      $scope.items = ['Apple', 'Banana', 'Orange'];
      
      $scope.toggleContent = function() {
        $scope.showContent = !$scope.showContent;
      };
      
      $scope.displayMessage = function() {
        $scope.message = $scope.inputValue;
      };
    });
  </script>
  <style>
    .highlight {
      color: blue;
      font-weight: bold;
    }
  </style>
</head>

<body ng-controller="myController">
  <h1>AngularJS Directives Example</h1>
  
  <button ng-click="toggleContent()">Toggle Content</button>
  <div ng-show="showContent">
    <h2>Repeating Items:</h2>
    <ul>
      <li ng-repeat="item in items">{{ item }}</li>
    </ul>
  </div>
  
  <input type="text" ng-model="inputValue">
  <button ng-click="displayMessage()">Display Message</button>
  <p ng-if="message">Entered value: <span ng-class="{ 'highlight': message }">{{ message }}</span></p>
</body>

</html>
```

In this example, we define an AngularJS application module named "myApp" and a controller named "myController." The controller contains the necessary logic and functions for handling the directives.

The `ng-click` directive is used to bind the `toggleContent()` function to the click event of the "Toggle Content" button. Clicking the button will toggle the value of the `showContent` variable, which determines whether the content should be displayed.

The `ng-show` directive is used to conditionally show or hide the content based on the value of `showContent`.

The `ng-repeat` directive is used to iterate over the `items` array and display each item in an unordered list.

The `ng-model` directive is used to bind the value of the `<input>` element to the


## Receiving events

```html
<!DOCTYPE html>
<html ng-app="myApp">

<head>
  <meta charset="UTF-8">
  <title>AngularJS SPA</title>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
  <style>
    .highlight {
      background-color: yellow;
    }
  </style>
  <script>
    var app = angular.module('myApp', []);

    app.controller('myController', function($scope) {
      $scope.inputValue = "";
      $scope.showContent = false;

      $scope.handleClick = function() {
        if ($scope.inputValue) {
          $scope.showContent = true;
        } else {
          $scope.showContent = false;
        }
      };
    });
  </script>
</head>

<body ng-controller="myController">
  <h1>AngularJS Single-Page Application</h1>
  <input type="text" ng-model="inputValue" placeholder="Enter some text">
  <button ng-click="handleClick()">Submit</button>

  <div ng-class="{'highlight': showContent}">
    <p ng-if="showContent">You entered: {{ inputValue }}</p>
  </div>
</body>

</html>
```

## Validating a form

```html
<!DOCTYPE html>
<html ng-app="myApp">

<head>
  <meta charset="UTF-8">
  <title>AngularJS Form Validation Example</title>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
  <script>
    var app = angular.module('myApp', []);

    app.controller('myController', function($scope) {
      $scope.user = {
        name: '',
        email: '',
        password: ''
      };

      $scope.submitForm = function() {
        if ($scope.myForm.$valid) {
          alert('Form submitted successfully!');
          // Additional logic for form submission
        } else {
          alert('Please correct the errors in the form!');
        }
      };
    });
  </script>
  <style>
    .error {
      color: red;
    }
  </style>
</head>

<body ng-controller="myController">
  <h1>AngularJS Form Validation Example</h1>
  
  <form name="myForm" novalidate>
    <div>
      <label for="name">Name:</label>
      <input type="text" id="name" name="name" ng-model="user.name" required>
      <span class="error" ng-show="myForm.name.$touched && myForm.name.$error.required">
        Name is required
      </span>
    </div>
    
    <div>
      <label for="email">Email:</label>
      <input type="email" id="email" name="email" ng-model="user.email" ng-pattern="/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/"
        required>
      <span class="error" ng-show="myForm.email.$touched && myForm.email.$error.required">
        Email is required
      </span>
      <span class="error" ng-show="myForm.email.$touched && myForm.email.$error.pattern">
        Invalid email format
      </span>
    </div>
    
    <div>
      <label for="password">Password:</label>
      <input type="password" id="password" name="password" ng-model="user.password" required minlength="8">
      <span class="error" ng-show="myForm.password.$touched && myForm.password.$error.required">
        Password is required
      </span>
      <span class="error" ng-show="myForm.password.$touched && myForm.password.$error.minlength">
        Password must be at least 8 characters long
      </span>
    </div>
    
    <button type="button" ng-click="submitForm()" ng-disabled="myForm.$invalid">Submit</button>
  </form>
</body>

</html>

```

## Making AJAX calls

```html
<!DOCTYPE html>
<html ng-app="myApp">

<head>
  <meta charset="UTF-8">
  <title>AngularJS AJAX Example</title>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
  <script>
    var app = angular.module('myApp', []);

    app.controller('myController', function($scope, $http) {
      $scope.userId = '';
      $scope.posts = [];

      $scope.searchPosts = function() {
        if ($scope.userId.trim() !== '') {
          var apiUrl = 'https://jsonplaceholder.typicode.com/posts?userId=' + encodeURIComponent($scope.userId);

          $http.get(apiUrl)
            .then(function(response) {
              $scope.posts = response.data;
            })
            .catch(function(error) {
              console.error('Error retrieving posts:', error);
              $scope.posts = [];
            });
        }
      };
    });
  </script>
</head>

<body ng-controller="myController">
  <h1>AngularJS AJAX Example</h1>

  <div>
    <input type="text" ng-model="userId" placeholder="Enter a user ID">
    <button type="button" ng-click="searchPosts()">Search</button>
  </div>

  <div ng-if="posts.length > 0">
    <h2>User Posts:</h2>
    <ul>
      <li ng-repeat="post in posts">
        <h3>{{ post.title }}</h3>
        <p>{{ post.body }}</p>
      </li>
    </ul>
  </div>

  <div ng-if="posts.length === 0">
    <p>No posts found.</p>
  </div>
</body>

</html>
```

In this example, we have an AngularJS application module named "myApp" and a controller named "myController." The controller contains a `$scope.userId` variable to hold the user's input (user ID) and a `$scope.posts` array to store the retrieved posts.

The `searchPosts` function is triggered when the user clicks the "Search" button. It constructs the API URL based on the user ID entered by the user and makes an AJAX call using the `$http` service's `get` method. The response from the JSONPlaceholder API is stored in the `$scope.posts` array.

The HTML template includes an input field where the user can enter a user ID and a button to initiate the search. The posts are displayed using the `ng-repeat` directive, which iterates over the `posts` array and generates a list item for each post. The post's title and body are displayed within the list item. If no posts are found, a corresponding message is displayed.

This example uses the JSONPlaceholder API (https://jsonplaceholder.typicode.com/posts) to retrieve posts based on the user ID. The API returns a collection of posts in JSON format. You can replace the API URL or modify the code to suit your specific requirements or use other endpoints provided by JSONPlaceholder API.

