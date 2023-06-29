# blogBUSTER frontend

## View

To start a routed Angular project named "blogBUSTER-frontend" follow these steps:

1. Set up a new Angular project:
Open a command prompt or terminal and run the following command to create a new Angular project:
```bash
ng new blogBUSTER-frontend --routing
```

2. Define types for Post and Page:
Create a new TypeScript file called `post.model.ts` in the `src/app` directory and define the types for Post and Page:
```typescript
// src/app/post.model.ts

export interface Post {
  id: number;
  title: string;
  body: string;
  date: string;
  labels: string;
  visible: boolean;
}

export interface Page<T> {
  content: T[];
  totalPages: number;
  totalElements: number;
  last: boolean;
  size: number;
  number: number;
  first: boolean;
  numberOfElements: number;
}
```

3. Create a service:
Generate a new service called `post.service.ts` using the Angular CLI command:
```bash
ng generate service post
```
Replace the content of the generated `post.service.ts` file in the `src/app` directory with the following code:
```typescript
// src/app/post.service.ts

import { Injectable } from '@angular/core';
import { HttpClient, HttpParams } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Post, Page } from './post.model';

@Injectable({
  providedIn: 'root'
})
export class PostService {
  private apiUrl = '/api/posts'; // Replace with your API URL

  constructor(private http: HttpClient) { }

  getPost(id: number): Observable<Post> {
    return this.http.get<Post>(`${this.apiUrl}/${id}`);
  }

  getPage(pageNumber: number, pageSize: number, orderBy: string, orderDirection: 'asc' | 'desc'): Observable<Page<Post>> {
    let params = new HttpParams()
      .set('page', pageNumber.toString())
      .set('size', pageSize.toString())
      .set('orderBy', orderBy)
      .set('orderDirection', orderDirection);

    return this.http.get<Page<Post>>(`${this.apiUrl}`, { params });
  }
}
```

4. Create a menu component:
Generate a new component called `menu` using the Angular CLI command:
```bash
ng generate component menu
```
To make the menu component path sensitive, you can modify the menu component in Angular as follows:

1. Open the `menu.component.ts` file in the `src/app/menu` directory (if it doesn't exist, create it).
2. Replace the default content with the following code:
```typescript
// src/app/menu/menu.component.ts

import { Component } from '@angular/core';
import { Router, NavigationEnd } from '@angular/router';

@Component({
  selector: 'app-menu',
  templateUrl: './menu.component.html',
  styleUrls: ['./menu.component.css']
})
export class MenuComponent {
  currentPath: string;

  constructor(private router: Router) {
    this.router.events.subscribe((event) => {
      if (event instanceof NavigationEnd) {
        this.currentPath = event.url;
      }
    });
  }
}
```

3. Open the `menu.component.html` file in the `src/app/menu` directory.
4. Replace the existing content with the following code:

```html
<!-- src/app/menu/menu.component.html -->

<nav>
  <ul>
    <li [class.active]="currentPath === '/'">
      <a routerLink="/">Home</a>
    </li>
    <li [class.active]="currentPath === '/posts'">
      <a routerLink="/posts">Posts</a>
    </li>
    <li [class.active]="currentPath === '/about'">
      <a routerLink="/about">About</a>
    </li>
    <!-- Add more menu items as needed -->
  </ul>
</nav>
```

5. Save the files.

In this implementation, the `MenuComponent` subscribes to the `NavigationEnd` event emitted by the `Router` to update the `currentPath` property with the current URL. The menu items use property binding and the `[class.active]` attribute to conditionally apply the `active` class to the menu item whose `routerLink` matches the `currentPath`. This allows you to style the active menu item to indicate the current selection.

Now, when you include the `<app-menu></app-menu>` selector in your app's template (e.g., `app.component.html`), the menu component will display the active menu item based on the current route.

Remember to update the menu items and their respective `routerLink` values based on your application's routes and desired navigation structure.

After starting the Angular development server (`ng serve`), you should see the menu component rendered in the application, and the active menu item will have the `active` class applied when the corresponding route is active.

5. Create a `post-view` component:
Generate a new component called `post-view` using the Angular CLI command:
```bash
ng generate component post-view
```
Open the `post-view.component.ts` file in the `src/app/post-view` directory. Replace the default content with the following code:

```typescript
// src/app/post-view/post-view.component.ts

import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { PostService } from '../post.service';
import { Post } from '../post.model';

@Component({
  selector: 'app-post-view',
  templateUrl: './post-view.component.html',
  styleUrls: ['./post-view.component.css']
})
export class PostViewComponent implements OnInit {
  post: Post;

  constructor(
    private route: ActivatedRoute,
    private postService: PostService
  ) {}

  ngOnInit(): void {
    const postId = +this.route.snapshot.paramMap.get('id');
    this.getPost(postId);
  }

  getPost(id: number): void {
    this.postService.getPost(id)
      .subscribe((post: Post) => {
        this.post = post;
      });
  }
}
```

Open the `post-view.component.html` file in the `src/app/post-view` directory. Replace the existing content with the following code:

```html
<!-- src/app/post-view/post-view.component.html -->

<div *ngIf="post">
  <h2>{{ post.title }}</h2>
  <p>{{ post.body }}</p>
  <p>Created Date: {{ post.date }}</p>
  <p>Labels: {{ post.labels }}</p>
  <p>Visible: {{ post.visible }}</p>
</div>
```

6. Define the routes:
Update the `app-routing.module.ts` file in the `src/app` directory to define the routes for the components:
```typescript
// src/app/app-routing.module.ts

import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { PostViewComponent } from './post-view/post-view.component';

const routes: Routes = [
  { path: 'posts/:id', component: PostViewComponent },
  // Add other routes as needed
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

7. Update the `app.component.html` file:
Update the `app.component.html` file in the `src/app` directory with the following content:
```html
<!-- src/app/app.component.html -->

<app-menu></app-menu>
<router-outlet></router-outlet>
```

8. Test the application:
You can now start the Angular development server by running the following command:
```bash
ng serve
```
Navigate to `http://localhost:4200`

 in your web browser to see the application in action. You can customize the menu component, create additional components, and update the routing configuration to fit your specific requirements.

Remember to update the `apiUrl` in the `PostService` with the appropriate URL for your backend API.

Note: Make sure you have the necessary dependencies installed, including the Angular CLI (`npm install -g @angular/cli`) and the required packages (`npm install`).

## Showing visible as an icon

To implement a filter to show the "visible" field as an icon in the `post-view` component, you can follow these steps:

1. Open the `post-view.component.html` file in the `src/app/post-view` directory.
2. Replace the line `<p>Visible: {{ post.visible }}</p>` with the following code:

```html
<!-- src/app/post-view/post-view.component.html -->

<p>
  Visible:
  <i class="material-icons">{{ post.visible ? 'check' : 'close' }}</i>
</p>
```

In this implementation, the `material-icons` class is used from the Material Design icons library. The `{{ post.visible ? 'check' : 'close' }}` expression conditionally sets the icon to either 'check' or 'close' based on the value of the `visible` field in the `post` object.

Make sure you have the necessary stylesheets and dependencies imported to use the Material Design icons. You can add the following link in the `index.html` file inside the `<head>` section to include the Material Design icons:

```html
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```

Now, when you view the `post-view` component, the `Visible` field will be displayed as an icon, showing a checkmark icon if the post is visible (`true`) or a close icon if the post is not visible (`false`).

You can further customize the icon styles or use different icon libraries based on your preferences.

Remember to update the `post-view` component with the necessary imports and dependencies to use the Material Design icons or any other icon library of your choice.
