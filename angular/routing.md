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
