# Interceptors

Interceptors are a powerful feature that allows you to intercept HTTP requests and responses globally and apply transformations, logging, error handling, or other operations. Interceptors act as middleware for HTTP communication, and they can be used to modify requests and responses before they are sent or received by the application.

In Angular, interceptors are implemented using the `HttpInterceptor` interface, and you can create your custom interceptors to perform specific tasks for every HTTP request or response made by your application.

Here's how you can create an interceptor in Angular:

1. Create a new TypeScript file for your interceptor, e.g., `my-interceptor.ts`.

2. Implement the `HttpInterceptor` interface in your interceptor class and override the `intercept` method. The `intercept` method takes two arguments: the HTTP request and a `HttpHandler` representing the next interceptor in the chain.

Example of a basic interceptor that logs every outgoing request and the corresponding response:

```typescript
import { Injectable } from '@angular/core';
import {
  HttpInterceptor,
  HttpRequest,
  HttpHandler,
  HttpEvent,
} from '@angular/common/http';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class MyInterceptor implements HttpInterceptor {
  intercept(
    req: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {
    console.log('Request sent:', req.url);

    return next.handle(req).pipe(
      tap(
        (event) => {
          console.log('Response received:', event);
        },
        (error) => {
          console.error('Error occurred:', error);
        }
      )
    );
  }
}
```

3. Register your interceptor in the `AppModule` or the module where you want it to be active. You can do this by providing the interceptor in the `HTTP_INTERCEPTORS` multi-provider token.

Example of registering the interceptor in `AppModule`:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';

import { AppComponent } from './app.component';
import { MyInterceptor } from './my-interceptor';

@NgModule({
  imports: [BrowserModule, HttpClientModule],
  declarations: [AppComponent],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: MyInterceptor,
      multi: true,
    },
  ],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

Now, every HTTP request and response made by your application will go through the `MyInterceptor` and log the corresponding information in the console.

Interceptors can be used for various purposes, such as:

1. Adding authentication headers to every request.
2. Handling errors globally and displaying user-friendly error messages.
3. Modifying request payloads or URLs before sending them.
4. Implementing caching mechanisms for responses.
5. Handling loading indicators or progress bars during requests.
6. Logging requests and responses for debugging purposes.

By using interceptors, you can keep your HTTP-related logic centralized and apply common functionalities consistently throughout your Angular application.

## Interceptor for JWT auth

To send an authentication JWT token in requests using an interceptor, you need to create a custom interceptor that will add the JWT token to the `Authorization` header of every outgoing HTTP request. Here's a step-by-step guide on how to achieve this:

1. Create a new TypeScript file for your interceptor, e.g., `auth-interceptor.ts`.

2. Implement the `HttpInterceptor` interface in your interceptor class and override the `intercept` method. The `intercept` method takes two arguments: the HTTP request and a `HttpHandler` representing the next interceptor in the chain.

3. In the `intercept` method, get the JWT token from your authentication service (wherever you store it after login) and add it to the `Authorization` header of the outgoing HTTP request.

4. If the JWT token is available, clone the original request and modify the cloned request's headers to include the token.

Example of an authentication interceptor:

```typescript
import { Injectable } from '@angular/core';
import {
  HttpInterceptor,
  HttpRequest,
  HttpHandler,
  HttpEvent,
} from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor(private authService: AuthService) {}

  intercept(
    req: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {
    const token = this.authService.getAuthToken(); // Implement getAuthToken in your AuthService
    if (token) {
      // Clone the request and add the Authorization header
      const authReq = req.clone({
        setHeaders: { Authorization: `Bearer ${token}` },
      });
      return next.handle(authReq);
    } else {
      // If no token, proceed with the original request
      return next.handle(req);
    }
  }
}
```

5. Register your interceptor in the `AppModule` or the module where you want it to be active. You can do this by providing the interceptor in the `HTTP_INTERCEPTORS` multi-provider token.

Example of registering the interceptor in `AppModule`:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule, HTTP_INTERCEPTORS } from '@angular/common/http';

import { AppComponent } from './app.component';
import { AuthInterceptor } from './auth-interceptor';

@NgModule({
  imports: [BrowserModule, HttpClientModule],
  declarations: [AppComponent],
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: AuthInterceptor,
      multi: true,
    },
  ],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

Now, with the `AuthInterceptor` registered, every HTTP request made by your application will have the JWT token added to the `Authorization` header, ensuring that authenticated requests are sent with the necessary authentication token.

Remember to implement the `getAuthToken` method in your `AuthService`, which should return the JWT token once the user is logged in. The implementation of the `AuthService` and where you store the token may vary depending on your application's authentication mechanism.
