# Routing

In a traditional web application, when a user navigates to a new URL, the browser sends a request to the web server to fetch the corresponding HTML, CSS, and JavaScript files. This process is known as a full page reload, and it can be time-consuming and lead to a poor user experience.

However, in an Angular application with routing, the browser doesn't perform a full page reload when the user changes the URL. Instead, Angular intercepts the URL change and handles it within the client-side application itself. Here's how it works:

1. Initial Application Load: When the user loads the Angular application for the first time, the browser fetches the initial HTML, CSS, and JavaScript files from the web server.

2. Single-Page Application (SPA) Concept: Angular transforms the application into a single-page application (SPA) by dynamically rendering and updating the content based on the requested routes without reloading the entire page.

3. Routing within the Application: When the user changes the URL, Angular's routing system intercepts the URL change and matches the requested route against the defined routes in the application.

4. Component Rendering: Angular's router renders the corresponding component's template associated with the matched route.

5. View Update: The component's template is rendered within the application's layout, replacing any previous content. Only the necessary data and UI changes are applied, rather than fetching and rendering the entire page.

By managing the routing and rendering process internally, Angular avoids the need for the browser to query the web server for new HTML files. Instead, it fetches the required data from APIs or local data sources and dynamically updates the view within the same page, resulting in a faster and more responsive user experience. Overall, Angular's routing mechanism enables seamless navigation within the application without the need for full page reloads, providing a smooth user experience while reducing the network traffic and latency associated with querying the web server for each URL change.

So, when a user changes the URL in the browser, Angular's routing system kicks in and performs the following steps:

1. URL Parsing: Angular's router parses the URL to identify the route and any route parameters.

2. Route Matching: The router matches the parsed URL against the defined routes in the application's routing configuration.

3. Component Rendering: Once a route is matched, Angular's router renders the corresponding component's template associated with the route.

4. Component Initialization: The component is initialized, and any necessary data retrieval or initialization logic can be executed.

5. View Update: The component's template is rendered within the `<router-outlet>` in the application's layout, replacing any previous content.

6. Route Events: Angular triggers various route-related events, such as `NavigationStart`, `NavigationEnd`, `NavigationCancel`, or `NavigationError`, allowing you to perform additional actions or handle specific scenarios.

7. History Manipulation: The router updates the browser's URL and history using the HTML5 History API, allowing users to use the browser's back and forward buttons for navigation.

By utilizing Angular's routing capabilities, you can create a single-page application experience where users can navigate between different views without full page reloads. The routing system manages the synchronization between the URL and the corresponding components, making it easier to build complex navigation flows and provide a seamless user experience. Angular's routing features include support for route parameters, query parameters, child routes, lazy loading, route guards for authentication and authorization, and more. It provides a flexible and powerful mechanism for handling navigation within your Angular applications.

## Setting up routing in an Angular application

Setting up routing in an Angular application involves several steps. Here's a guide to help you set up routing:

1. Generate an Angular routing module:
   - Use the Angular CLI command `ng generate module app-routing --flat --module=app` to generate a routing module. This will create a file named `app-routing.module.ts` in your project's root directory.

2. Configure routes in the routing module:
   - Open the `app-routing.module.ts` file and import the necessary modules:
     ```typescript
     import { NgModule } from '@angular/core';
     import { Routes, RouterModule } from '@angular/router';
     ```
   - Define your routes as an array of `Route` objects:
     ```typescript
     const routes: Routes = [
       { path: '', redirectTo: '/home', pathMatch: 'full' },
       { path: 'home', component: HomeComponent },
       { path: 'about', component: AboutComponent },
       { path: 'products', component: ProductsComponent },
       { path: 'products/:id', component: ProductDetailComponent },
       { path: '**', component: PageNotFoundComponent },
     ];
     ```
   - In the `routes` array, each route object consists of a `path` property and a corresponding `component` property. You can also specify other properties like `redirectTo` or `children` for nested routes.
   - Customize the routes based on your application's requirements.

3. Add the `RouterModule` to the `imports` array:
   - Still in the `app-routing.module.ts` file, add the `RouterModule` to the `imports` array, and configure it with your defined routes:
     ```typescript
     @NgModule({
       imports: [RouterModule.forRoot(routes)],
       exports: [RouterModule]
     })
     export class AppRoutingModule { }
     ```

4. Set up routing in the root module:
   - Open the root module file (usually `app.module.ts`) and import the `AppRoutingModule`:
     ```typescript
     import { AppRoutingModule } from './app-routing.module';
     ```
   - Add the `AppRoutingModule` to the `imports` array:
     ```typescript
     @NgModule({
       imports: [
         // other imports
         AppRoutingModule
       ],
       // other module configurations
     })
     export class AppModule { }
     ```

5. Add a `<router-outlet>` to your application's template:
   - In your root component's template file (usually `app.component.html`), add a `<router-outlet>` element where the content of each route will be displayed:
     ```html
     <router-outlet></router-outlet>
     ```

6. Add navigation links in your application:
   - In your component templates or navigation menus, use the `routerLink` directive to create links to navigate to different routes:
     ```html
     <a routerLink="/home">Home</a>
     <a routerLink="/about">About</a>
     <a [routerLink]="['/products', productId]">Product Details</a>
     ```

With these steps, you have set up basic routing in your Angular application. The `routerLink` directive allows users to navigate between different routes, and the `<router-outlet>` displays the content of the currently active route. You can further customize and enhance your routing configuration based on your application's needs, such as adding nested routes, route guards, lazy loading, and more.

## Configuring routes and route parameters

Configuring routes and route parameters in the `app-routing.module.ts` file involves defining the routes and their corresponding components, as well as specifying any required route parameters. Here's a detailed explanation of how to configure routes and route parameters in the `app-routing.module.ts` file:

1. Import Required Modules and Components:
   - At the top of the file, import the necessary modules and components that will be used in the routing configuration:
     ```typescript
     import { NgModule } from '@angular/core';
     import { RouterModule, Routes } from '@angular/router';
     import { HomeComponent } from './home.component';
     import { AboutComponent } from './about.component';
     import { ProductComponent } from './product.component';
     ```

2. Define Routes:
   - Inside the `@NgModule` decorator, define the routes using the `Routes` type:
     ```typescript
     const routes: Routes = [
       { path: '', redirectTo: '/home', pathMatch: 'full' },
       { path: 'home', component: HomeComponent },
       { path: 'about', component: AboutComponent },
       { path: 'products/:id', component: ProductComponent },
     ];
     ```

3. Configure Route Parameters:
   - If you need to define route parameters, you can specify them in the route path using the `:parameterName` syntax. For example, in the above code snippet, `:id` represents a route parameter.
   - The route parameters can be accessed in the corresponding component using the `ActivatedRoute` service.
   - You can also define multiple route parameters by separating them with slashes, such as `products/:category/:id`.

4. Handle Default and Redirect Routes:
   - You can define a default route by using an empty path and specifying the `redirectTo` property with the desired route path. For example, `{ path: '', redirectTo: '/home', pathMatch: 'full' }` redirects the empty path to the `/home` route.
   - The `pathMatch: 'full'` ensures that the entire URL must match the specified path for the redirection to occur.

5. Export the Routing Module:
   - At the bottom of the file, export the routing module using the `RouterModule.forRoot()` method, passing in the defined routes:
     ```typescript
     @NgModule({
       imports: [RouterModule.forRoot(routes)],
       exports: [RouterModule]
     })
     export class AppRoutingModule { }
     ```

6. Import and Add the Routing Module:
   - In the root module (`app.module.ts`), import the `AppRoutingModule` and add it to the `imports` array:
     ```typescript
     import { AppRoutingModule } from './app-routing.module';
     
     @NgModule({
       imports: [
         // other imports
         AppRoutingModule
       ],
       // other module configurations
     })
     export class AppModule { }
     ```

With these steps, you have configured the routes and route parameters in the `app-routing.module.ts` file. The defined routes will map specific paths to the corresponding components, and route parameters can be accessed and used within those components. You can further customize the routing configuration by adding additional routes, child routes, route guards, lazy loading, and more, depending on the requirements of your Angular application.


## Navigation

In Angular, you can navigate between different views and components using routing. Angular's routing system allows you to define routes and associate them with specific components. Here's how you can navigate between views and components using routing:

1. Set up Routes:
   - In your `app-routing.module.ts` file (or any custom routing module), define the routes for your application. Each route consists of a `path` and a corresponding `component`. For example:
     ```typescript
     const routes: Routes = [
       { path: '', redirectTo: '/home', pathMatch: 'full' },
       { path: 'home', component: HomeComponent },
       { path: 'about', component: AboutComponent },
       { path: 'products/:id', component: ProductComponent },
     ];
     ```

2. Create Navigation Links:
   - In your template files, create navigation links using the `routerLink` directive. For example:
     ```html
     <a routerLink="/home">Home</a>
     <a routerLink="/about">About</a>
     <a [routerLink]="'/products/' + productId">Product</a>
     ```

3. Use Router Outlet:
   - In your main application template file (usually `app.component.html`), add the `<router-outlet></router-outlet>` tag where the component corresponding to the active route will be rendered. For example:
     ```html
     <h1>My Application</h1>
     <nav>
       <a routerLink="/home">Home</a>
       <a routerLink="/about">About</a>
       <a [routerLink]="'/products/' + productId">Product</a>
     </nav>
     <router-outlet></router-outlet>
     ```

4. Handle Programmatic Navigation:
   - In addition to using navigation links, you can also navigate programmatically in response to user actions or events. To do this, you need to inject the `Router` service into your component and use its methods. For example:
     ```typescript
     import { Router } from '@angular/router';
     
     constructor(private router: Router) {}
     
     navigateToHome() {
       this.router.navigate(['/home']);
     }
     
     navigateToProduct(productId: string) {
       this.router.navigate(['/products', productId]);
     }
     ```

5. Route Parameters:
   - If you have defined route parameters, you can pass them while navigating. For example:
     ```typescript
     this.router.navigate(['/products', productId]);
     ```

By following these steps, you can navigate between different views and components using Angular's routing system. Clicking on navigation links or programmatically invoking the router's navigation methods will update the URL and render the corresponding component in the `<router-outlet>`. Route parameters can be used to pass dynamic data between components. Angular's routing system takes care of rendering and updating the views, making it easy to create multi-page applications with seamless navigation.

## Receiving route parameters in components

To receive route parameters in an Angular component from the URL, you can use the `ActivatedRoute` service provided by Angular's Router module. The `ActivatedRoute` service gives you access to the current route and its parameters. Here's how you can receive route parameters in a component:

1. Import the Required Modules and Services:
   - In your component file, import the necessary modules and services:
     ```typescript
     import { Component, OnInit } from '@angular/core';
     import { ActivatedRoute } from '@angular/router';
     ```

2. Inject the ActivatedRoute Service:
   - In the component's constructor, inject the `ActivatedRoute` service:
     ```typescript
     constructor(private route: ActivatedRoute) { }
     ```

3. Access the Route Parameters:
   - Inside the component, you can access the route parameters using the `params` observable provided by the `ActivatedRoute` service.
   - In the `ngOnInit()` method (or any other lifecycle hook), subscribe to the `params` observable to retrieve the route parameters:
     ```typescript
     ngOnInit() {
       this.route.params.subscribe(params => {
         const id = params['id']; // Access the 'id' route parameter
         // Use the parameter value in your component logic
         console.log('Received ID:', id);
       });
     }
     ```

4. Use the Route Parameters in Component Logic:
   - Once you have accessed the route parameter(s), you can use them in your component's logic, such as making API calls, fetching data, or updating the view based on the received parameter value.

By following these steps, you can receive route parameters in an Angular component from the URL. The `ActivatedRoute` service provides access to the current route's parameters through the `params` observable. Subscribing to the `params` observable allows you to retrieve the route parameters and use them within your component's logic.

## Route Components or View Components vs Child Components or Embedded Components

1. Route Components or View Components:
   - These are the components that are associated with specific routes in your application.
   - They represent the views or pages that users navigate to.
   - Route components are defined in the routing configuration and are rendered within the `<router-outlet>` based on the current route.
   - They receive route parameters and can access them through the `ActivatedRoute` service.
   - Examples include HomeComponent, AboutComponent, ProductDetailComponent, etc.

2. Child Components or Embedded Components:
   - These are the components used within other components or templates.
   - They serve as building blocks to enhance the functionality or modularity of the parent component.
   - Child components are instantiated and rendered based on their usage in the parent component's template.
   - They receive input data from the parent component through property bindings.
   - Examples include ModalComponent, FormComponent, SidebarComponent, etc.

It's important to note that the terms "route components" and "child components" are not official Angular terminologies but are commonly used in the Angular community to describe these concepts. These terms help differentiate components based on their usage and purpose within the application's structure.

## Route guards

Route guards in Angular are used to control access to routes based on certain conditions or criteria. They allow you to prevent navigation to a route, redirect to a different route, or perform additional checks before allowing access to a route. Route guards are implemented as services and can be attached to individual routes or applied globally to all routes.

There are three types of route guards in Angular:

1. CanActivate: This guard determines whether a user can access a specific route. It is used to check if the user has the necessary permissions, authentication, or any other criteria before allowing access. If the guard returns `true`, navigation to the route is allowed. If it returns `false` or a `UrlTree` object, navigation is canceled or redirected to a different route. Multiple `CanActivate` guards can be applied to a single route, and all guards must return `true` for the route to be accessible.

2. CanActivateChild: This guard is similar to `CanActivate` but is applied to child routes. It checks if the user has permission to access the child routes within a parent route. It works in a similar way to `CanActivate`, allowing or preventing navigation based on the guard's result.

3. CanDeactivate: This guard is used when leaving a route to determine if the user can navigate away from the current route. It can be used to prompt the user for confirmation or perform any necessary checks before allowing navigation. The `CanDeactivate` guard is associated with a component and is triggered when the user attempts to navigate away from that component. It returns a boolean, `Observable<boolean>`, or `Promise<boolean>` to allow or prevent navigation.

To implement route guards, you need to create a service that implements the corresponding guard interface (`CanActivate`, `CanActivateChild`, or `CanDeactivate`). The service can then be attached to the route configuration using the `canActivate`, `canActivateChild`, or `canDeactivate` properties.

Here's an example of a simple `CanActivate` guard:

```typescript
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, UrlTree, Router } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private router: Router) {}

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean | UrlTree {
    // Check if the user is authenticated
    const isAuthenticated = ... // Perform authentication check

    if (isAuthenticated) {
      return true; // Allow access to the route
    } else {
      // Redirect to the login page
      return this.router.createUrlTree(['/login'], { queryParams: { returnUrl: state.url } });
    }
  }
}
```

In the above example, the `AuthGuard` implements the `CanActivate` interface and checks if the user is authenticated. If the user is authenticated, it returns `true`, allowing access to the route. Otherwise, it creates a `UrlTree` object that redirects the user to the login page, passing the current route URL as a query parameter.

Route guards provide a powerful way to control access and perform additional checks before allowing navigation in your Angular application. They can be used to enforce authentication, authorization, or any other custom logic you require.

# Content Expansion 

* Implementing lazy loading of modules.



