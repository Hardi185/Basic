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

