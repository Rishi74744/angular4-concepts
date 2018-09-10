# Decorators in Angular 4

## What are decorators?
Decorators provide a manner which can be used to define annotations and meta-programming syntax for class declarations and its members (parameter, accessor, method or property). Decorators are available as experimental feature of typescript and are a Stage 2 proposal for Javascript.

There are mainly four types of decorators in angular 4 -
1. Class Decorators - @Component and @NgModule
2. Parameter Decorators - @Inject
3. Property Decorators - @Input and @Output
4. Method Decorators - @HostListener

## @Component Decorator -
A component is what defines an angular application. It is the basic building block of the application. When you define angular component, it provides some properties to specify the metadata required for the processing of component like - selector, templateUrl, styleUrls etc.
Here is how you declare a component in angular 4 -

```
@Component({
    selector: 'test-component',
    templateUrl: './test.component.html',
    styleUrls:['./test.component.css']
})
export class TestComponent {

}
```
Component must declare selector and templateUrl or template.
Now, when you want use this component within your application, you just need to import the component and add it to the declarations in NgModule as -

```
import {TestComponent} from './test-component';

@NgModule({
  declarations: [
    TestComponent
  ],
  imports: [

  ],
  providers: [],
  bootstrap: []
})
```
A component must belong to a NgModule in order to be used by other components.

Before we move to @NgModule decorator, let us have a look at the core concept of Angular 2+ framework - Dependency Injection.

### Dependency Injection -
It allows a class to receive dependencies from other classes. DI or Dependency Injection in angular in achieved by injecting a service class into the component.

Here is how you make a service injectable -

```
import { Injectable } from '@angular/core';

@Injectable
export class TestService {
  constructor(){
    console.log("Test Service");
  }
  getTest(){
    console.log("Get Test Method!");
  }
}
```

We inject the service dependency in component class as follows -

```
import { Component } from '@angular/core';
import { TestService } from './test.service';

@Component{
  selector: 'test-component',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css'],
  providers: [TestService]
}

export class TestComponent {
  constructor(private testService: TestService) {
  }

  callTest() {
    this.testService.getTest();
  }

}
```

## @NgModule Decorator -
NgModule is a way to organize the application dependencies. It allows us to group the features together. It wraps up components, filters, pipes, directives etc and making them available as per the need. NgModule is declared as follows -

```
@NgModule({
    declarations: [
        AppComponent,
        TestComponent1,
        TestComponent2,
        TestComponent3,
        TestDirective1,
        TestDirective2,
        TestPipe1,
        TestPipe2
    ],
    imports: [
        BrowserModule
    ],
    providers: [
        TestService1,
        TestService2
    ],
    exports: [
        TestComponent1,
        TestComponent2
    ],
    bootstrap: [AppComponent]
})
export class AppModule { }
```

**declarations** - Declarations are private by default. These are the components, directives and pipes that are used by the module.

**imports** - Imports are the modules on which this module depends on.

**exports** - Exports declared are publicly available.

**providers** - They are the injectables or services. Eg - Http Calls, Business Logic

**bootstrap** - The component defined within this will be bootstrapped when this module is bootstrapped. This component is usually the root component.

## @Injectable Decorator -
When we use @Injectable with a class then it means that the class can be used with dependency injector. There is no need to use @Injectable if there is no dependency within service. But if there is any dependency within the Service then we need to add @Injectable.

```
export class TestService {
  constructor(){
    console.log("Test Service");
  }
}

import { Component } from '@angular/core';
import { TestService } from './test.service';

@Component{
  selector: 'test-component',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css'],
  providers: [TestService]
}

export class TestComponent {
  constructor(private testService: TestService) {
  }

  callTest() {
    this.testService.getTest();
  }

}
```

This will work fine. But if we change service class as follows -

```
export class TestService {
  constructor(httpService: HttpService){
    console.log("Test Service");
  }
}
```

We will get an error : ** Can't resolve all parameters for TestService: (?).** Now, we need to add the @Injectable -
```
@Injectable
export class TestService {
  constructor(httpService: HttpService){
    console.log("Test Service");
  }
}
```

## @Inject Decorator -
It is a manual way of specifying that a parameter needs to be injected. It can get difficult to manage dependencies as they increase for a component. We can inject parameter as -

```
import { Component, Inject } from '@angular/core';
import { ChatWidget } from '../components/chat-widget';

@Component({
  selector: 'test-component',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent {
  constructor(@Inject(Http) private http) { }
}
```

In the above code, we have made http a singleton angular associate with class Http.

## @Input Decorator -
If we want to pass data from parent component to child component, then we use @Input. This can be achieved as -

```
import { Component, Input } from '@angular/core';

@Component({
  selector: 'test-child-component',
  template: `
    <h3>{{testName}}</h3>
    <p>I am {{testJob}}</p>
  `
})
export class TestChildComponent {
  @Input() testName: String;
  @Input('job') testJob: String;
}

import { Component } from '@angular/core';

@Component({
  selector: 'test-parent-component',
  template: `
    <h2>User Name and Job</h2>
    <test-child-component
      [testName]="testName"
      [job]="job">
    </test-child-component>
  `
})
export class TestParentComponent {
  testName = "Test123";
  job = 'Developer';
}
```

## @Output Decorator -
It is used when we want our component to emit an event. We can emit an event using @Output and EventEmitter. It is used when we want to call a method of parent class when a particular event occurs in child class, say click of a button or change of drop down value. Here is how -

```

import { Component, EventEmitter, Output } from '@angular/core';

@Component({
    selector: 'test-child-component',
    template: `<button (click)="toggleValue()">Click me</button> `
})
export class TestChildComponent {

    @Output() toggleValue = new EventEmitter();
    isValid: boolean = false;

    toggleValue() {
        this.isValid = !this.isValid;
        this.toggleValue.emit(this.isValid);
    }
}


import { Component, OnInit } from '@angular/core';

@Component({
    selector: 'test-parent-component',
    template: `<test-child-component (toggleValue)='displayState($event)'></test-child-component>`
})
export class TestParentComponent {

    displayState(isValid) {
        console.log("State is : " + isValid);
    }
}
```

## @HostListener Decorator -
This decorator allows you to handle events of the host element in the directive class. Directives helps us to extend the HTML with new attributes. It allows us to attach a custom behaviour to DOM.

```
import { Directive, HostListener } from '@angular/core';

@Directive({
    selector: '[testDirective]'
})
export class TestDirective {

    constructor() {
    }

    @HostListener('mouseover') onMouseOver() {
      console.log('Mouse Over Event');
    }

    @HostListener('click') onClick() {
      console.log('Click Event');
    }
}
```
