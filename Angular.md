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
