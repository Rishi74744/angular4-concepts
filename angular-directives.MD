# Directives in angular
There are three types of directives in angular 4 -
1.) Structural Directives
2.) Attribute Directives
3.) Component

Components in angular 4 are directives with a template. Structural and Attribute directives does not have a template.
Structural directives manipulate DOM by adding or removing DOM elements where as Attribute directives change the appearance and behaviour of th DOM.

## Structural Directives -
They are prefixed with an asterisk (*) which conveys information to Angular that structure of DOM may change within the directive. The most commonly used built-in angular directives are -
1.) *ngIf
2.) *ngFor

## *ngIf -
*ngIf directive is used when we want to manipulate the DOM based on some condition. Here's how -

```
<div *ngIf="isValid">
  <h3>This is a test div. It will be added to DOM only when isValid is "true".</h3>
</div>
```

## *ngFor -
*ngFor directive is used when we want to add multiple DOM elements depending on the number of elements present in the iterable. Here's how we user *ngFor -

```
<div *ngFor="let user of userList">
  <div>{{user.userName}}</div>
  <div>{{user.address}}</div>
  <div>{{user.email}}</div>
  <div>{{user.mobile}}</div>
</div>
```

This will add n number of DOM elements with above structure, where n is the number of users present in the userList.

## Attribute Directives -
Attribute directives are surrounded by brackets and can change the appearance or behaviour of DOM within the directive based on any specific condition. Some of the in built angular attribute directives are -
1.) [hidden]
2.) [ngClass]
3.) [ngStyle]

They are used as -

```
<div>
  <div [hidden] = "!isLoading">Hide while loading</div>
  <div [ngCLass] = "{active: isActive}">active css class is applied to div isAcive is true</div>
  <div [ngStyle] = "addCss('testItem')">addCss will be called to get the css for 'testItem'</div>
</div>

export class TestComponent {
  addCss(item) {
      let styles = {
        'font'  : item? '24px solid bold'   : 'none',
        'visibility' : !item? 'hidden' : 'visible'
      }
      return styles;
   }
}
```

## Creating Custom Directives in Angular 4 -
To create a custom directive in angular, follow steps below -

```
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[testDir]',
  host: {
    '(keyup)': 'onKeyUp()',
    '(keydown)': 'onKeyDown()'
  }
})
export class TestDirective {
  private element: HTMLElement;

  constructor(elementRef: ElementRef){
    this.element = elementRef.nativeElement;
  }

  onKeyUp() {
     console.log("On Key Up Called");
   }
  onKeyDown() {
    console.log("On Key Down Called");
   }

}

import { Component } from '@angular/core';
import { TestDirective } from './test.directive'

@Component({
  selector: 'test-app',
  template: '<div><input type="text" value="Write in me" testDir/></div>',
  directives: [TestDirective]
})
export class AppComponent { }
```

ElementRef provides us the DOM reference of the element to which the directive is attached to.  
**selector**  - defines the name of directive that will be used when applying to the dom.
**host** - defines methods to call on different events.

AppComponent class template shows how to use the custom directive. This is how we create custom directive in angular 4 and use it in our component.
