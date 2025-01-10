# Angular Overview

## What is Angular?

Angular is a platform and framework that allows us to build client-side applications using **HTML**, **CSS**, **JavaScript**, and **TypeScript**.

---

## Main Features of Angular

1. **Two-Way Data Binding**
   - Sync data between the model and the view automatically, ensuring that changes in one are reflected in the other.

2. **Dependency Injection**
   - Efficiently manages and injects dependencies, enhancing modularity and simplifying testing.

3. **Component-Based Approach**
   - Breaks down the application into smaller, reusable components, making it easier to develop, maintain, and scale.

4. **Templating with Directives**
   - Angular provides powerful directives such as:
     - `*ngFor`: Used for iterating over a collection.
     - `*ngIf`: Conditionally displays elements.
     - `*ngSwitch`: Handles conditional rendering based on multiple conditions.

5. **RESTful API Handling**
   - Simplifies interaction with RESTful services and APIs using tools like the `HttpClient` module.

6. **Lazy Loading**
   - Delays the loading of modules until they are required, reducing the initial bundle size and improving application performance.

---

## What is a Component in Angular?
A **component** is the basic building block of an Angular application. It consists of HTML, CSS, and TypeScript files and can be reused across the application.

### Key Features:
- Defined and configured using the `@Component` decorator.
- Combines logic, templates, and styles.
- Encapsulation of functionality and design.

### Example Component:
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-header', // Custom HTML tag: <app-header>
  templateUrl: './header.component.html', // External HTML template
  styleUrls: ['./header.component.css'], // External CSS files
  encapsulation: ViewEncapsulation.Emulated // Controls style encapsulation (optional)
})
export class HeaderComponent {
  title: string = 'My Angular App'; // Component logic
}
```

### Key Properties of `@Component`:
1. **`selector`**:
   - Defines the custom HTML tag for the component.
   - Example: `selector: 'app-header'` can be used as `<app-header></app-header>`.

2. **`template` or `templateUrl`**:
   - **`template`**: Inline HTML content.
     ```typescript
     template: `<h1>{{ title }}</h1>`
     ```
   - **`templateUrl`**: Links to an external HTML file.
     ```typescript
     templateUrl: './header.component.html'
     ```

3. **`styles` or `styleUrls`**:
   - **`styles`**: Inline CSS styles.
     ```typescript
     styles: [`h1 { color: blue; }`]
     ```
   - **`styleUrls`**: Links to external CSS files.
     ```typescript
     styleUrls: ['./header.component.css']
     ```

4. **`encapsulation`**:
   - Controls the scope of CSS styles:
     - `Emulated` (default): Scoped styles.
     - `None`: Global styles.
     - `ShadowDom`: True encapsulation using Shadow DOM.

5. **`providers`**:
   - Declares services specific to the component.
     ```typescript
     providers: [SomeService]
     ```

6. **`animations`**:
   - Adds animations to the component.
     ```typescript
     animations: [trigger('fadeIn', ...)]
     ```

### Using a Component:
1. Declare the component in the module (e.g., `AppModule`).
2. Use its selector in the HTML.

**Example:**
```typescript
@NgModule({
  declarations: [HeaderComponent], // Declare the component
  bootstrap: [AppComponent]
})
export class AppModule {}
```
```html
<!-- AppComponent HTML -->
<app-header></app-header>
```

**Rendered Output:**
```html
<h1>My Angular App</h1>
```

### Benefits of Angular Components:
1. **Reusability**: Create reusable UI elements.
2. **Modularity**: Break down applications into manageable parts.
3. **Encapsulation**: Isolate logic, template, and styles.
4. **Testability**: Test components independently.

---

## What is a Module in Angular?
An **Angular module** is a logical container for grouping related components, directives, pipes, and services. It helps organize the application into cohesive blocks.

### The `NgModule` Decorator:
The `@NgModule` decorator is used to define a module and configure its metadata.

### Example Module:
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { HeaderComponent } from './header/header.component';

@NgModule({
  declarations: [ // Components, directives, and pipes
    AppComponent,
    HeaderComponent
  ],
  imports: [ // Other modules
    BrowserModule
  ],
  providers: [ // Services
  ],
  bootstrap: [ // Root component
    AppComponent
  ]
})
export class AppModule {}
```

### Key Metadata of `@NgModule`:
1. **`declarations`**:
   - Specifies components, directives, and pipes that belong to the module.
   - Example: `declarations: [AppComponent, HeaderComponent]`.

2. **`imports`**:
   - Specifies other modules to be used.
   - Example: `imports: [BrowserModule]`.

3. **`providers`**:
   - Registers services available in the module.
   - Example: `providers: [SomeService]`.

4. **`bootstrap`**:
   - Specifies the root component to bootstrap the application.
   - Example: `bootstrap: [AppComponent]`.

---

## What is Angular CLI?

The Angular CLI (Command Line Interface) is a powerful tool provided by Angular to streamline the development process. It automates repetitive tasks, ensuring consistency and reducing human error.

### Features of Angular CLI

1. **Code Generation:** Quickly generates components, directives, pipes, services, modules, and more using commands.
   ```bash
   ng generate component my-component
   ```

2. **Project Scaffolding:** Initializes a fully structured Angular project with a single command.
   ```bash
   ng new my-angular-app
   ```

3. **Development Server:** Launches a local development server with live reload support.
   ```bash
   ng serve
   ```

4. **Build Optimization:** Builds production-ready applications with optimizations like bundling, minification, and Ahead-of-Time (AOT) compilation.
   ```bash
   ng build --prod
   ```

5. **Testing:** Runs unit and end-to-end tests for Angular applications.
   ```bash
   ng test
   ng e2e
   ```

6. **Linting:** Ensures code quality by running linting checks.
   ```bash
   ng lint
   ```

7. **Third-party Integration:** Easily adds third-party libraries and tools to the project.
   ```bash
   ng add @angular/material
   ```

8. **Environment Configuration:** Manages multiple environment setups like development, staging, and production.
   ```bash
   ng build --configuration production
   ```

9. **File Watching:** Automatically recompiles the project on file changes during development.

### Basic Commands
| Command                   | Description                                              |
|---------------------------|----------------------------------------------------------|
| `ng new <project-name>`   | Creates a new Angular project with default settings.     |
| `ng generate component`   | Generates a new component and updates the module.        |
| `ng generate service`     | Creates a new service and updates the module.            |
| `ng serve`                | Starts the development server with live reload.          |
| `ng build`                | Compiles the application into a distributable format.    |
| `ng test`                 | Runs unit tests for the application.                    |
| `ng e2e`                  | Executes end-to-end tests for the application.           |
| `ng add <library>`        | Installs and integrates a library into the project.      |
| `ng lint`                 | Analyzes the project code for linting errors.            |

---

## What is a Directive in Angular?

A directive in Angular adds behavior to elements in the DOM.

### Types of Directives

1. **Structural Directives:**
   Modify the structure of the DOM by adding or removing elements.
   - Examples: `*ngIf`, `*ngFor`, `*ngSwitch`.

   ```html
   <div *ngIf="isLoggedIn">Welcome, User!</div>
   <ul>
     <li *ngFor="let item of items">{{ item }}</li>
   </ul>
   ```

2. **Attribute Directives:**
   Modify the appearance or behavior of an existing DOM element.
   - Examples: `ngClass`, `ngStyle`, custom attribute directives.

   ```html
   <div [ngClass]="{'active': isActive}">Dynamic class</div>
   <button [ngStyle]="{'background-color': 'blue', 'color': 'white'}">Styled Button</button>
   ```

### Creating a Custom Directive

1. Generate a Directive:
   ```bash
   ng generate directive highlight
   ```

2. Directive Code:
   ```typescript
   import { Directive, ElementRef, HostListener, Renderer2 } from '@angular/core';

   @Directive({
     selector: '[appHighlight]'
   })
   export class HighlightDirective {
     constructor(private el: ElementRef, private renderer: Renderer2) { }

     @HostListener('mouseenter') onMouseEnter() {
       this.renderer.setStyle(this.el.nativeElement, 'background-color', 'yellow');
     }

     @HostListener('mouseleave') onMouseLeave() {
       this.renderer.removeStyle(this.el.nativeElement, 'background-color');
     }
   }
   ```

3. Use the Directive:
   ```html
   <p appHighlight>Hover over me to see the effect!</p>
   ```

---

## What is a Service in Angular?

A service in Angular is a class that provides specific functionality or logic that can be shared across components, directives, or other services. Services handle business logic, data management, HTTP requests, utility functions, and more.

### Creating and Using a Service

1. Generate a Service:
   ```bash
   ng generate service data
   ```

2. Service Example:
   ```typescript
   import { Injectable } from '@angular/core';
   import { HttpClient } from '@angular/common/http';
   import { Observable } from 'rxjs';

   @Injectable({
     providedIn: 'root'  // Makes the service globally available
   })
   export class DataService {

     constructor(private http: HttpClient) { }

     getData(): Observable<any> {
       return this.http.get('https://api.example.com/data');
     }

     saveData(data: any): Observable<any> {
       return this.http.post('https://api.example.com/save', data);
     }
   }
   ```

3. Inject the Service into a Component:
   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { DataService } from './data.service';

   @Component({
     selector: 'app-data',
     templateUrl: './data.component.html',
     styleUrls: ['./data.component.css']
   })
   export class DataComponent implements OnInit {

     data: any[];

     constructor(private dataService: DataService) { }

     ngOnInit(): void {
       this.dataService.getData().subscribe(response => {
         this.data = response;
       });
     }
   }
   ```

4. Use the Service in the Template:
   ```html
   <ul>
     <li *ngFor="let item of data">{{ item.name }}</li>
   </ul>
   ```

---

## Observables and `subscribe()`

### Understanding Observables
Observables in Angular are like a YouTube channel:
- **Observable**: The channel keeps emitting new videos (data).
- **Subscriber**: You subscribe to the channel to get updates (like observing the data).
- **Notification**: Hitting the bell icon ensures you get notified (using `subscribe()` in Angular).

So any time data is read, observable will add it in that stream, any new data or modification will directly be added to stream, and if we’ve subscribed that already, the block of code inside that will run every time.

### Why Use Observables for HTTP Requests in Angular?
HTTP requests are asynchronous, and Observables allow efficient handling of responses:
- **Asynchronous**: Emit values when data is available.
- **Lazy**: Start emitting values only when subscribed.
- **Composable**: Combine, filter, and transform using RxJS operators.

### `subscribe()` Arguments
- **Next**: Handles each emitted value.
- **Error**: Handles errors during emission.
- **Complete**: Executes when the Observable completes.

#### Observable Flow:
- **Creation:** The getData() method creates an Observable that will emit the HTTP response.
- **Subscription:** The component subscribes to the Observable to receive the HTTP data.


### Example
```typescript
// Service method returning an Observable
getData(): Observable<any> {
  return this.http.get('https://api.example.com/data');
}

// Component calling getData() and subscribing to the Observable
this.dataService.getData().subscribe(
  response => {
    this.data = response; // Handle emitted data
  },
  error => {
    console.error('An error occurred:', error); // Handle errors
  },
  () => {
    console.log('Data fetching completed'); // Handle completion
  }
);
```

### Data Emission with Modifications or Changes
- Observable emits data whenever there is an update.
- For example, if you are tracking a list of users, and new users are added or existing users are modified in the backend, every time the data changes, the Observable emits the new data.
- If the Observable is "listening" to this stream, it will notify the subscriber (via subscribe()), and the subscriber’s callback will run.

---

## Two-Way Data Binding
Two-way data binding synchronizes data between the component class and the view (template). This ensures data flows in both directions.

### How It Works
1. **Component to View (Property Binding)**: When a property in the component changes, that change is reflected in the view. This happens through `property binding` (one-way data flow).
2. **View to Component (Event Binding)**: When the user interacts with the view (e.g., input changes), that change is reflected in the component. This happens through ` event binding` (one-way data flow from the view to the component).
   
### Example
**Component (TypeScript):**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-name-input',
  templateUrl: './name-input.component.html',
  styleUrls: ['./name-input.component.css']
})
export class NameInputComponent {
  name: string = ''; // Property bound to the input field
}
```

**Template (HTML):**
```html
<!-- Two-way data binding using ngModel -->
<input [(ngModel)]="name" placeholder="Enter your name">
<p>Your name is: {{ name }}</p>
```

### Explanation
1. **Initial Value**: `name` starts as an empty string.
2. **Property Binding**: Updates the input field when `name` changes.
3. **Event Binding**: Updates `name` when the user types in the input field.

---

## AngularJS vs Angular

AngularJS is a JavaScript-based framework developed by Google for building dynamic single-page web applications (SPAs)

| **Feature**            | **AngularJS**                            | **Angular (2+)**                                   |
|-------------------------|------------------------------------------|---------------------------------------------------|
| **Language**           | JavaScript                              | TypeScript (superset of JavaScript)              |
| **Architecture**       | MVC (Model-View-Controller)             | Component-based architecture                     |
| **Data Binding**       | Two-way data binding                    | Supports two-way data binding, but is more efficient |
| **Dependency Injection** | Basic DI system                        | Advanced and more flexible DI system             |
| **Routing**            | Uses `ngRoute` for routing              | Uses `@angular/router` with advanced features like lazy loading |
| **Performance**        | Slower, especially for larger apps      | Faster due to optimizations like AOT compilation and better change detection |
| **Mobile Support**     | Limited mobile support                  | Improved mobile support and optimized for performance |
| **Directives**         | Heavy use of directives (e.g., `ng-model`) | Less reliance on directives, focused on components |
| **Version Support**    | Only one version (1.x)                  | Continually updated with new versions (Angular 2, 4, 5, 6, etc.) |
| **Development Speed**  | Slower (due to legacy features)         | Faster, thanks to Angular CLI and modern tooling |
| **Testability**        | Testable but more complex               | Improved testability with better structure and TypeScript |
| **Learning Curve**     | Easier for beginners (less structure)   | Steeper learning curve due to TypeScript and component-based design |
| **Mobile and Desktop Apps** | Not optimized for mobile            | Fully optimized for mobile (PWA) and desktop apps |

---

## Dependency Injection
Dependency Injection (DI) decouples components and services in Angular, providing a scalable architecture.

DI helps decouple components and services. The component does not need to know how to create or manage the service; it just needs to rely on the injected dependency.

### How DI Works
1. **Define a Dependency**: Create a service.
2. **Provide the Dependency**: You register this dependency in Angular’s dependency injection system (either globally in the root or at a module level).
3. **Inject the Dependency**: Angular automatically injects the dependency into a class that needs it, usually via the constructor.

### Example
**Creating a Service:**
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root' // Service available globally
})
export class UserService {
  getUserInfo() {
    return { name: 'John Doe', age: 30 };
  }
}
```

**Injecting the Service:**
```typescript
import { Component, OnInit } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-user-profile',
  template: `<h1>{{ userInfo.name }}</h1>`
})
export class UserProfileComponent implements OnInit {
  userInfo: any;

  constructor(private userService: UserService) { }

  ngOnInit() {
    this.userInfo = this.userService.getUserInfo();
  }
}
```

**How Dependency Injection Works:**
- When UserProfileComponent is created, Angular's DI system automatically provides an instance of UserService and injects it into the constructor.
- You don't need to manually create an instance of UserService (e.g., new UserService()). Angular handles that for you.


### Injector Scopes
1. **Root Injector**: Singleton, shared across the application.
2. **Module Injector**: Scoped to a specific module.
3. **Component Injector**: Scoped to a specific component and its children.

**Providing at Different Levels:**
- **Global (Root):**
  ```typescript
  @Injectable({
    providedIn: 'root'
  })
  export class GlobalService { }
  ```
- **Module-Level:**
  ```typescript
  @NgModule({
    providers: [ModuleService]
  })
  export class SomeModule { }
  ```
- **Component-Level:**
  ```typescript
  @Component({
    selector: 'app-user-profile',
    providers: [UserProfileService]
  })
  export class UserProfileComponent { }
  ```
  
---


  
  
