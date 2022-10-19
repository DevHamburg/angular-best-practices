# AngularBestPractices

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 14.2.6.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.

# Software Requirements

Node.js: https://nodejs.org

Angular CLI: https://cli.angular.io

VS Code: https://code.visualstudio.com (_Any editor can be used_)

# Planning the application architecture

- Architecture considerations
- Architecture planning template
- The Angular Style Guide
- other considerations

## Architecture considerations

You can find a template inside the docs folder

- App Overview
- App Features
- Domain Security
- Domain Rules
- Logging
- Services/Communication
- Data Model
- Feature Components
- Shared Functionality

## The Angular Style Guide

- Coding conventions
- Naming rules
- Organizing modules
- Creating and using components
- Creating and using services
- Lifecycle hooks

https://angular.io/guide/styleguide

## Other Considerations

- Accessibility
- i18n
- Environments
- CI / CD
- CDN, container, server, cloud
- Unit testing
- End-to-end testing
- APIs
- More...

## Summary

- Take time to discuss and document ke architecture concepts
- Create a simple architecture template that can be reused across projects
- Create your own style guide that's based on the official Angular style guide

# Organizing Features and Modules

- Organizing features
- Feature modules
- Core and shared modules
- Creating a custom library
- Consuming a custom library

## Organizing Features

Do structure the app such that you can **Locate** code quickly, **Identify** the code at a glance, keep the **Flattest** structure you can, and **Try** to be **DRY**.

**LIFT:** Locate, Identify, Flattest and Try to be Dry

### **Convention-based**

- Follow strict naming conventions
- Related code may be seperated

_Can result in a lot of files in a folder in larger applications_

### **Feature-based** _(recommented)_

- Features are organized into their own folder
- Features are self-contained
- Easy to find everything related to a feature

Use a feature-based approach to structure your code

Use the Angular CLI to generate initial feature folder / component

Minimum of one module per feature (as appropriate)

Avoid deeply nested folders

Flatten feature hierarchies

## Feature Modules

**Customers**

- Feature routing module
- Feature module

**Orders**

- Feature routing module
- Feature module

## Core and Shared Modules

### **Core Module**

- Core folder should contain singleton services shared throughout app
- Services that are specific to a feature can go in the features's folder

**Example:** LoggingService, ErrorService, DataService

### **Shared Module**

- Shared folder should contain reusable components, pipes, directives

**Example:** CalendarComponent, AutoCompleteComponent

### Importing Core and Shared

Core Module -> Root Module (only)

Shared Module -> Root Module, Feature Module, ... (everywhere where it is needed)

**Preventing Reimport of Core**
Core should only be imported into the root/app module

```
export function throwIfAlreadyLoaded(parentModule: any, moduleName: string) {
  if (parentModule) {
    throw new Error(`${moduleName} has already been loaded. Import Core modules in the AppModule only.`);
  }
}
```

How to use

```
import { throwIfAlreadyLoaded } from './import.guard';

export class CoreModule  {
  constructor( @Optional() @SkipSelf() parentModule: CoreModule) {
    throwIfAlreadyLoaded(parentModule, 'CoreModule');
  }
}
```

@Optinal(): _A constrcutor parameter decorator that marks a dependency as optional._

@Optional if there is not a parent module

@SkipSelf(): _A constrcutor parameter decorator that tells the DI (Dependency Injection) framework that dependency should start from the parent injector._

**Alternative Approach - Base Class**

```
export class EnsureModuleLoadedOnceGuard {

  constructor(targetModule: any) {
    if (targetModule) {
      throw new Error(`${targetModule.constructor.name} has already been loaded. Import this module in the AppModule only.`)
    }
  }
}
```

Core Module

```
import { EnsureModuleLoadedOnceGuard } from './ensure-module-loaded-once.guard';

export class CoreModule extrends EnsureModuleLoadedOnceGuard {
  constructor( @Optional() @SkipSelf() parentModule: CoreModule) {
    super(parentModule);
  }
}
```

## Creating a Custom Library
