Summary
-------

Angular cheat sheet - original from https://github.com/angular/angular/blob/master/aio/content/guide/cheatsheet.md

## Setup and Startup

```
ng new --minimal=true   \                       // Minal app setup - less opined
       --skipTests=true \
       myapp

ng update @angular/core @angular/cli            // Update to latest Angular - eg Aio7 to Aio8
```

## Control where features are added

```
$ mkdir dashboard
$ cd dashboard
$ ng g m dashboard --routing --flat   # add feature module in current folder
$ ng g c summary/Summary --flat       # add your components
$ ng g c detail/Detail --flat         # add another component
```

## Add Proxy to API - Avoid CORS

If your running an API on a dev sentence (eg Flask, Django, Simplerr) that would normally run on the same server you can proxy it through ng-serve. Create a script in package.json and start the project with `npm start` as follows:

```
    # file: proxy.conf.json
    {
      "**": {
        "target": "http://localhost:9000",
        "secure": false,
        "logLevel": "debug",
        "changeOrigin": true
      }
    }


    # file: package.json
      "scripts": {
        ...
        "start": "ng serve --proxy-config proxy.conf.json",
        ...
```

## Template Syntax

```
<input [value]="firstName">                     // Binds property value to the result of expression firstName.

<div [attr.role]="myAriaRole">                  // Binds attribute role to the result of expression myAriaRole.

<div [class.extra-sparkle]="isDelightful">      // Binds the presence of the CSS class extra-sparkle on the element
                                                //  to the truthiness of the expression isDelightful.

<div [style.width.px]="mySize">                 // Binds style property width to the result of expression mySize in
                                                //  pixels. Units are optional.

<button (click)="readRainbow($event)">          // Calls method readRainbow when a click event is triggered on this
                                                //  button element (or its children) and passes in the event object.

<div title="Hello {{ponyName}}">                // Binds a property to an interpolated string, for example, "Hello Seabiscuit".
                                                //  Equivalent to: <div [title]="'Hello ' + ponyName">

<p>Hello {{ponyName}}</p>                       // Binds text content to an interpolated string, for example, "Hello Seabiscuit".

<my-cmp [(title)]="name">                       // Sets up two-way data binding. Equivalent to:
                                                //  <my-cmp [title]="name" (titleChange)="name=$event">

<video #movieplayer ...>                        // Creates a local variable movieplayer that provides access to the video
    <button (click)="movieplayer.play()">       //  element instance in data-binding and event-binding expressions in the
</video>                                        //  current template.

<p *myUnless="myExpression">...</p>             // The * symbol turns the current element into an embedded template. Equivalent
                                                //  to: <ng-template [myUnless]="myExpression"><p>...</p></ng-template>

<p>Card No.: {{number | numberFormatter}}</p>   // Transforms the current value of expression cardNumber via the pipe
                                                //  called myCardNumberFormatter.

<p>Employer: {{employer?.companyName}}</p>      // The safe navigation operator (?) means that the employer field is optional
                                                //  and if undefined, the rest of the expression should be ignored.

<svg:rect x="0" y="0" width="100"               // An SVG snippet template needs an svg: prefix on its root element to
    height="100"/>                              //  disambiguate the SVG element from an HTML component.


<svg> <rect x="0" y="0" width="100"             // An <svg> root element is detected as an SVG element automatically,
    height="100"/></svg>                        //  without the prefix.

```

## Built-in directives

```
import { CommonModule } from '@angular/common';

<section *ngIf="showSection">                   // Removes or recreates a portion of the DOM tree based on the showSection
                                                //  expression.

<li *ngFor="let item of list">                  // Turns the li element and its contents into a template, and uses that to
                                                // instantiate a view for each item in list.

<div [ngSwitch]="conditionExpression">          // Conditionally swaps the contents of the div by selecting one of the
    <ng-template [ngSwitchCase]="case1Exp">     //  embedded templates based on the current value of conditionExpression.
        ...
    </ng-template>

    <ng-template ngSwitchCase="litstring">
        ...
    </ng-template>

    <ng-template ngSwitchDefault>
        ...
    </ng-template>
</div>

<div [ngClass]="{'active': isActive,            // Binds the presence of CSS classes on the element to the truthiness of
        'disabled': isDisabled}">               //  the associated map values. The right-hand expression should
                                                //  return {class-name: true/false} map.


<div [ngStyle]="{'property': 'value'}">         // Allows you to assign styles to an HTML element using CSS. You can use
<div [ngStyle]="dynamicStyles()">               //  CSS directly, as in the first example, or you can call a method from the component.
```

## Forms

```
import { FormsModule } from '@angular/forms';

<input [(ngModel)]="userName">                  // Provides two-way data-binding, parsing, and validation for form controls.
```

## Class decorators

```
import { Directive, ... } from '@angular/core';

@Component({...})                               // Declares that a class is a component and provides metadata about the component.
class MyComponent() {}

@Directive({...})                               // Declares that a class is a directive and provides metadata about the directive.
class MyDirective() {}

@Pipe({...})                                    // Declares that a class is a pipe and provides metadata about the pipe.
class MyPipe() {}

@Injectable()                                   // Declares that a class has dependencies that should be injected into the
class MyService() {}                            //  constructor when the dependency injector is creating an instance of this class.
```

## Directive configuration

```
@Directive({ property1: value1, ... })

selector: '.cool-button:not(a)'                 // Specifies a CSS selector that identifies this directive within a template.
                                                //  Supported selectors include element, [attribute], .class, and :not().
                                                //
                                                // Does not support parent-child relationship selectors.

providers: [MyService, { provide: ... }]        // List of dependency injection providers for this directive and its children.
```

## Component configuration

```
@Component extends @Directive, so the @Directive configuration applies to components as well

moduleId: module.id                             // If set, the templateUrl and styleUrl are resolved relative to the component.

viewProviders: [MyService, { provide: ... }]    // List of dependency injection providers scoped to this component's view.

template: 'Hello {{name}}'                      // Inline template or external template URL of the component's view.
templateUrl: 'my-component.html'

styles: ['.primary {color: red}']               // List of inline CSS styles or external stylesheet URLs for styling the component’s view.
styleUrls: ['my-component.css']
```

## Class field decorators for directives and components

```
import { Input, ... } from '@angular/core';

@Input() myProperty;                            // Declares an input property that you can update via property
                                                //  binding (example: <my-cmp [myProperty]="someExpression">).

@Output() myEvent = new EventEmitter();         // Declares an output property that fires events that you can subscribe to with
                                                //  an event binding (example: <my-cmp (myEvent)="doSomething()">).

@HostBinding('class.valid') isValid;            // Binds a host element property (here, the CSS class valid) to a directive/component
                                                //  property (isValid).

@HostListener('click',                          // Subscribes to a host element event (click) with a directive/component
    ['$event']) onClick(e) {...}                //  method (onClick), optionally passing an argument ($event).


@ContentChild(myPredicate) myChildComponent;    // Binds the first result of the component content query (myPredicate) to a
                                                //  property (myChildComponent) of the class.

@ContentChildren(myPredicate) myChildComponents;// Binds the results of the component content query (myPredicate) to a
                                                //  property (myChildComponents) of the class.

@ViewChild(myPredicate) myChildComponent;       // Binds the first result of the component view query (myPredicate) to a
                                                //  property (myChildComponent) of the class. Not available for directives.

@ViewChildren(myPredicate) myChildComponents;   // Binds the results of the component view query (myPredicate) to a
                                                //  property (myChildComponents) of the class. Not available for directives.
```

## Directive and component change detection and lifecycle hooks (implemented as class methods)

```
constructor(myService: MyService, ...) { ... }  // Called before any other lifecycle hook. Use it to inject dependencies, but
                                                //  avoid any serious work here.

ngOnChanges(changeRecord) { ... }               // Called after every change to input properties and before processing content
                                                //  or child views.

ngOnInit() { ... }                              // Called after the constructor, initializing input properties, and the first
                                                //  call to ngOnChanges.

ngDoCheck() { ... }                             // Called every time that the input properties of a component or a directive
                                                //  are checked. Use it to extend change detection by performing a custom check.

ngAfterContentInit() { ... }                    // Called after ngOnInit when the component's or directive's content has been initialized.

ngAfterContentChecked() { ... }                 // Called after every check of the component's or directive's content.

ngAfterViewInit() { ... }                       // Called after ngAfterContentInit when the component's views and child views / the
                                                //  view that a directive is in has been initialized.

ngAfterViewChecked() { ... }                    // Called after every check of the component's views and child views / the view
                                                //  that a directive is in.

ngOnDestroy() { ... }                           // Called once, before the instance is destroyed.
```

## Dependency injection configuration

```
{ provide: MyService, useClass: MyMockService } // Sets or overrides the provider for MyService to the MyMockService class.

{ provide: MyService, useFactory: myFactory }   // Sets or overrides the provider for MyService to the myFactory factory function.

{ provide: MyValue, useValue: 41 }              // Sets or overrides the provider for MyValue to the value 41.
```

## Routing and navigation

```
import { Routes, RouterModule, ... } from '@angular/router';


// Configures routes for the application. Supports static, parameterized,
//  redirect, and wildcard routes. Also supports custom route data and resolve.
const routes: Routes = [
    { path: '', component: HomeComponent },
    { path: 'path/:routeParam', component: MyComponent },
    { path: 'staticPath', component: ... },
    { path: '**', component: ... },
    { path: 'oldPath', redirectTo: '/staticPath' },
    { path: ..., component: ..., data: { message: 'Custom' } }]);

const routing = RouterModule.forRoot(routes);


// Marks the location to load the component of the active route.
<router-outlet>
</router-outlet>

<router-outlet name="aux">
</router-outlet>


// Creates a link to a different view based on a route instruction consisting of a
//  route path, required and optional parameters, query parameters, and a fragment.
//  To navigate to a root route, use the / prefix; for a child route, use the
//  ./prefix; for a sibling or parent, use the ../ prefix.
<a routerLink="/path">
<a [routerLink]="[ '/path', routeParam ]">
<a [routerLink]="[ '/path', { matrixParam: 'value' } ]">
<a [routerLink]="[ '/path' ]" [queryParams]="{ page: 1 }">
<a [routerLink]="[ '/path' ]" fragment="anchor">


// The provided classes are added to the element when the routerLink becomes
// the current active route.
<a [routerLink]="[ '/path' ]" routerLinkActive="active">


// An interface for defining a class that the router should call first to
//  determine if it should activate this component. Should return a boolean|UrlTree
//  or an Observable/Promise that resolves to a boolean|UrlTree.
class CanActivateGuard implements CanActivate {
    canActivate( route: ActivatedRouteSnapshot,
        state: RouterStateSnapshot ): Observable<boolean|UrlTree>|Promise<boolean|UrlTree>|boolean|UrlTree { ... }
}{ path: ..., canActivate: [CanActivateGuard] }


// An interface for defining a class that the router should call first to
//  determine if it should deactivate this component after a navigation. Should
//  return a boolean|UrlTree or an Observable/Promise that resolves to a
//  boolean|UrlTree.
class CanDeactivateGuard implements CanDeactivate<T> {
    canDeactivate( component: T,
                    route: ActivatedRouteSnapshot,
                    state: RouterStateSnapshot ): Observable<boolean|UrlTree>|Promise<boolean|UrlTree>|boolean|UrlTree {
                        ...
                    }
}{ path: ..., canDeactivate: [CanDeactivateGuard] }


// An interface for defining a class that the router should call first to
//  determine if it should activate the child route. Should return a
//  boolean|UrlTree or an Observable/Promise that resolves to a boolean|UrlTree.
class CanActivateChildGuard implements CanActivateChild {
    canActivateChild( route: ActivatedRouteSnapshot,
                        state: RouterStateSnapshot ): Observable<boolean|UrlTree>|Promise<boolean|UrlTree>|boolean|UrlTree {
                            ...
                        }
}{ path: ..., canActivateChild: [CanActivateGuard], children: ... }


// An interface for defining a class that the router should call first to resolve
//  route data before rendering the route. Should return a value or an
//  Observable/Promise that resolves to a value.
class ResolveGuard implements Resolve<T> {
    resolve( route: ActivatedRouteSnapshot,
                state: RouterStateSnapshot ): Observable<any>|Promise<any>|any {
                    ...
                }
}{ path: ..., resolve: [ResolveGuard] }


// An interface for defining a class that the router should call first to check if
//  the lazy loaded module should be loaded. Should return a boolean|UrlTree or an
//  Observable/Promise that resolves to a boolean|UrlTree.
class CanLoadGuard implements CanLoad {
    canLoad( route: Route ): Observable<boolean|UrlTree>|Promise<boolean|UrlTree>|boolean|UrlTree {
        ...
        }
}{ path: ..., canLoad: [CanLoadGuard], loadChildren: ... }
```

## Useful Design Patterns

### Simple mode-service api
```
# file: user.model.ts
export class User {
    id: number;
    username: string;
    password: string;
    firstName: string;
    lastName: string;
}

# file: user.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

import { User } from './user.model';

@Injectable()
export class UserService {
    constructor(private http: HttpClient) { }

    getAll() { return this.http.get<User[]>(`${config.apiUrl}/users`); }
    getById(id: number) { return this.http.get(`${config.apiUrl}/users/` + id); }
    register(user: User) { return this.http.post(`${config.apiUrl}/users/register`, user); }
    update(user: User) { return this.http.put(`${config.apiUrl}/users/` + user.id, user); }
    delete(id: number) { return this.http.delete(`${config.apiUrl}/users/` + id); }
}
```

### Simple http render

```
import { Component, OnInit } from '@angular/core';
import {Observable} from 'rxjs/Observable';
import {HttpClient} from '@angular/common/http';

@Component({
  selector: 'app-details',
  template: `
    <h1 *ngIf="detail$ | async as detail else noData">
        {{ detail.message }}
    </h1>
    <ng-template #naData>No Data! Or Details!</ng-template>
  `,
  styles: []
})
export class DashboardComponent implements OnInit {
    details$
    constructor(private http:HttpClient) { }

    ngOnInit() {
        this.detail$ = this.http
        .get(`${config.apiUrl}/detail`) //--> returns {message: 'Hello'}
    }
}

```

### Basic Authentication using angular2-jwt

src: https://ryanchenkie.com/angular-authentication-using-route-guards

Assumes that you are storing the user’s JWT in local storage.

Needs: `npm install --save @auth0/angular-jwt@beta`

```
// file://app/auth/auth.service.ts
import { Injectable } from '@angular/core';
import { JwtHelper } from '@auth0/angular-jwt';

@Injectable()
export class AuthService {

  constructor(public jwtHelper: JwtHelper) {}

  // ...
  public isAuthenticated(): boolean {

    const token = localStorage.getItem('token');

    // Check whether the token is expired and return
    // true or false
    return !this.jwtHelper.isTokenExpired(token);
  }
}


// file://app/auth/auth-guard.service.ts
import { Injectable } from '@angular/core';
import { Router, CanActivate } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable()
export class AuthGuardService implements CanActivate {

  constructor(public auth: AuthService, public router: Router) {}

  canActivate(): boolean {
    if (!this.auth.isAuthenticated()) {
      this.router.navigate(['login']);
      return false;
    }
    return true;
  }

}

// file://app/app.routes.ts
import { Routes, CanActivate } from '@angular/router';
import { ProfileComponent } from './profile/profile.component';
import { AuthGuardService as AuthGuard } from './auth/auth-guard.service';

export const ROUTES: Routes = [
  { path: '', component: HomeComponent },
  { path: 'profile', component: ProfileComponent, canActivate: [AuthGuard] },
  { path: '**', redirectTo: '' }
];

//
// With Roles - using jwt-decode
//

// file://app/auth/role-guard.service.ts
import { Injectable } from '@angular/core';
import { Router, CanActivate, ActivatedRouteSnapshot } from '@angular/router';
import { AuthService } from './auth.service';
import decode from 'jwt-decode';

@Injectable()
export class RoleGuardService implements CanActivate {

  constructor(public auth: AuthService, public router: Router) {}

  canActivate(route: ActivatedRouteSnapshot): boolean {

    // this will be passed from the route config
    // on the data property
    const expectedRole = route.data.expectedRole;

    const token = localStorage.getItem('token');

    // decode the token to get its payload
    const tokenPayload = decode(token);

    if (!this.auth.isAuthenticated() || tokenPayload.role !== expectedRole) {
      this.router.navigate(['login']);
      return false;
    }
    return true;
  }

}

// file://app/app.routes.ts
import { Routes, CanActivate } from '@angular/router';
import { ProfileComponent } from './profile/profile.component';
import { AuthGuardService as AuthGuard } from './auth/auth-guard.service';
import { RoleGuardService as RoleGuard } from './auth/role-guard.service';

export const ROUTES: Routes = [
  { path: '', component: HomeComponent },
  { path: 'profile', component: ProfileComponent, canActivate: [AuthGuard] },
  {
    path: 'admin',
    component: AdminComponent,
    canActivate: [RoleGuard],
    data: { expectedRole: 'admin' }
  },
  { path: '**', redirectTo: '' }
];
```
