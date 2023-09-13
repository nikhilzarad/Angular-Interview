# Angular-Interview

## Angular Service

**_What is a service?_**

It is Class which is used to create components or common functionality which can be shared
across the entire application.

**Example**

API call function and normal function which can accept parameter and return a value.

**_How to Use Service_**

ng generate service my-new-service: add a service to your
application

import { Injectable } from '@angular/core';
import { Http } from '@angular/http';

@Injectable({

> The Injectable decorator is required for dependency
> injection to work

> providedIn option registers the service with a specific NgModule

providedIn: 'root',

> This declares the service with the root app

(AppModule)})export class RepoService{

constructor(private http: Http){
}

fetchAll(){
return this.http.get('https://api.github.com/repositories');
}}

The above service uses Http service as a dependency.

**_What is dependency injection in Angular? _**

It is a class asks for dependencies from external
sources rather than creating them itself.

Dependency Injection is nothing but process of object creation into constructor of any class.

Means We create object of HttpClient into constructor as below is called Dependency Injection .

**_Example_**

WAY 1 - in service
import { HttpClient} from '@angular/common/http';  
 @Injectable({  
 providedIn: 'root',  
 })  
 export class MasterService {  
 constructor(private http: HttpClient) { } = > this is Dependency Injection
}
WAY 2 - in component

import { MasterService} from './master.service';  
 @Component({  
 selector: 'app-root',  
 templateUrl: './app.component.html',  
 })  
 export class AppComponent {  
 constructor(private master: MasterService){ } - > this is also depedency injection
}

## observable & observer

**_What is an observable?_**

It is provide support for passing messages between publishers and subscribers in your
application.

They are mainly used for event handling,asynchronous programming, and handling multiple values.
In this case, you Make a function for publishing values, but it is not
executed until a consumer subscribes to it.

The observables are created using new keyword

**Example**

import { Observable } from 'rxjs';

const observable = new Observable(observer => {

setTimeout(() => {
observer.next('Hello from a Observable!');

}, 2000);});

**_What is subscribing or observer ?_**

An Observable instance begins publishing values only when someone
subscribes to it. So you need to subscribe by calling
the subscribe() method of the instance, passing an observer object to
receive the notifications.

**Example**

Let's take an example of creating and subscribing to a simple
observable, with an observer that logs the received message to the
console

Creates an observable sequence of 5 integers, starting from 1

const source= range(1, 5);

> Create observer object

const myObserver = {

next: x => console.log('Observer got a next value: ' + x),

error: err => console.error('Observer got an error: ' + err),

complete: () => console.log('Observer got a complete notification'),};

> Execute with the observer object and Prints out each item

source.subscribe(myObserver);

> Observer got a next value: 1
> Observer got a next value: 2
> Observer got a next value: 3
> Observer got a next value: 4
> Observer got a next value: 5
> Observer got a complete notification

**_What is the difference between promise and observable? _**

**Observable** :Declarative: An Observable instance begins publishing values
only when someone subscribes to it.
**Promise**:Execute immediately on creation

**Observable**: Provide multiple values over time
**Promise**:Provide only one

**Observable**: Subscribe method is used for error handling
**Promise**:Push errors to the child promises

**_How do you perform error handling in observables?_**

You can handle errors by define an error callback on the observer
instead of try/catch Method

**Example**

myObservable.subscribe({

next(num) { console.log('Next num: ' + num)},

error(err) { console.log('Received an errror: ' + err)}});

## RxJS

**_What is RxJS? _**r

RxJS is a library for Write asynchronous and callback-based code
in a functional, reactive style using Observables.

Many APIs such as
HttpClient produce and consume RxJS Observables and also uses
operators for processing observables.

**Example**

you can import observables and operators for using
HttpClient as below,

import { Observable, throwError } from 'rxjs';import { catchError, retry }
from 'rxjs/operators';

**_What are the utility functions provided by RxJS?_**

i. Converting existing code for async operations into observables
ii. Iterating through the values in a stream
iii.Mapping values to different types
iv. Filtering streams

**_what is an rxjs subject in Angular_**

An RxJS Subject is a special type of Observable that allows values to be
multicasted to many Observers While plain Observables are unicast

A Subject is like an Observable, but can multicast to many Observers.

import { Subject } from 'rxjs';
const subject = new Subject<number>();
subject.subscribe({
next: (v) => console.log(`observerA: ${v}`)
});
subject.subscribe({
next: (v) => console.log(`observerB: ${v}`)
});

## HttpClient

**_What is HttpClient and its benefits?_**

Most of the Front-end applications communicate with backend services
over HTTP protocol using either XMLHttpRequest interface or the fetch() API.

import { HttpClientModule } from '@angular/common/http';

**_Explain on how to use HttpClient with an example?_**

> Import HttpClient into root module:

import { HttpClientModule } from '@angular/common/http';

@NgModule({
imports: [
BrowserModule,

> import HttpClientModule after BrowserModule.
> HttpClientModule,
> ],
> ......
> })

export class AppModule {}

> Inject the HttpClient into the application: Let's
> create a userProfileService(userprofile.service.ts) as an example. It also defines get method of
> HttpClient

import { Injectable } from '@angular/core';import { HttpClient }
from '@angular/common/http';

const userProfileUrl: string = 'assets/data/profile.json';

@Injectable()export class UserProfileService {

constructor(private http: HttpClient) { }

getUserProfile() {
return this.http.get(this.userProfileUrl);
}}

> Create a component for subscribing service: Let's
> create a component called UserProfileComponent(userprofile.component.ts) which inject UserProfileService and invokes the service method,

fetchUserProfile() {
this.userProfileService.getUserProfile()
.subscribe((data: User) => this.user = {
id: data['userId'],
name: data['firstName'],
city: data['city']
});}

> Since the above service method returns an Observable which needs to be subscribed in the component.

**_HTTP Interceptors_**

Interceptors are sitting in between your application and the backend. By using interceptors you can transform a request coming from the application before it is actually submitted to the backend. The same is possible for responses. If a response arrives from the backend the interceptor can transform that response before it arrives in your application.

**_HttpReuest_**

**GET**

HttpClient.get() method returns an Observable and lets you read the data from the server.

getUsers(): Observable<User> {
return this.http.get<User>(this.url);
}

**POST**

We must send a user object to server to post the user details. Hence, HttpClient.post() takes an argument user along with the url where the request body is to be sent and the headers (httpOptions)

postUsers(user): Observable<User> {
return this.http.post<User>(this.url, user, this.httpOptions);
}

**PUT**
This method is almost similar to post(). I’m updating a user record based on the id field. Hence, a new url is to be constructed based on userId so that no other objects are disturbed.

updateUser(userId, user): Observable<User> {
const putUrl = this.url + '/' + userId;
return this.http.put<User>(putUrl, user, this.httpOptions);
}

**DELETE**
I’m deleting a user object by capturing the userId. So, construct a new url which deletes the specific user based on the id.

deleteUser(userId): Observable<{}> {
const deleteUrl = this.url + '/' + userId;
return this.http.delete(deleteUrl, this.httpOptions);
}

## Angular Pipes

**_What are pipes? _**

A pipe takes in data as input and transforms it to a desired output.

For example, let us take a pipe to transform a component's birthday
property into a human-friendly date using date pipe.

import { Component } from '@angular/core';

@Component({
selector: 'app-birthday',
template: `<p>Birthday is {{ birthday | date }}</p>`})export class
BirthdayComponent {
birthday = new Date(1987, 6, 18);

> June 18, 1987}

**_What is a custom pipe? _**

Apart from built-inn pipes, you can write your own custom pipe

i. A pipe is a class decorated with pipe
metadata @Pipe decorator, which you import from
the core Angular library For example,
@Pipe({name: 'myCustomPipe'})

ii. The pipe class implements
the PipeTransform interface's transform method
that accepts an input value followed by optional
parameters and returns the transformed value.

interface PipeTransform {
transform(value: any, ...args: any[]): any}

iv. The @Pipe decorator allows you to define the pipe
name that you'll use within template expressions. It
must be a valid JavaScript identifier.

template: `{{someInputValue | myCustomPipe: someOtherValue}}`

**_example of custom pipe? _**

let us create a custom pipe for finding file size

i.
t import { Pipe, PipeTransform } from
'@angular/core';

@Pipe({name: 'customFileSizePipe'})
export class FileSizePipe implements PipeTransform {
transform(size: number, extension: string = 'MB'): string {
return (size / (1024 \* 1024)).toFixed(2) + extension;
}
}

> tofixed use for round the the result upto two decimal

ii.
Now you can use the above pipe in template expression as
below, javascript template:

`<h2>Find the size of a file</h2> <p>Size:
{{288966 | customFileSizePipe: 'GB'}}</p>`

**_What is a parameterized pipe? _**

The parameterized pipe can be created by declaring the pipe name with a colon ( : ) and then the parameter value. If the pipe accepts
multiple parameters, separate the values with colons.

Let's take a
birthday example with a particular format(dd/MM/yyyy):

import { Component } from '@angular/core';

@Component({
selector: 'app-birthday',
template: `<p>Birthday is {{ birthday | date:'dd/MM/yyyy'}}</p>`

> 18/06/1987
> })

export class BirthdayComponent {
birthday = new Date(1987, 6, 18);
}

**_How do you chain pipes? _**

You can chain pipes together combinations of pipes as per the needs.

Let's take a birthday property which uses date pipe(along
with parameter) and uppercase pipes as below

import { Component } from '@angular/core';

@Component({
selector: 'app-birthday',
template: `<p>Birthday is {{ birthday | date:'fullDate' |
uppercase}} </p>`

> THURSDAY, JUNE 18, 1987
> })
> export class BirthdayComponent {
> birthday = new Date(1987, 6, 18);
> }

**_What is the purpose of async pipe? _**

The AsyncPipe subscribes to an observable or promise and returns the
latest value it has emitted. When a new value is emitted, the pipe marks
the component to be checked for changes

**_Exapmle_**

Let's take a time observable which continuously updates the view for
every 2 seconds with the current time

@Component({
selector: 'async-observable-pipe',
template: `<div><code>observable|async</code>: Time: {{ time |
async }}</div>`})

export class AsyncObservablePipeComponent {
time = new Observable(observer =>
setInterval(() => observer.next(new Date().toString()), 2000)
);}

**_What is the difference between pure and impure pipe? _**
**pure pipe**
A pure pipe is only called when Angular detects a change in the
value or the parameters passed to a pipe.

For example, any changes to a primitive input value (String, Number, Boolean,Symbol) or a changed object reference (Date, Array, Function,
Object).

**Impure Pipe**

An impure pipe is called for every change detection cycle no matter whether the value or parameters changes.
For example,

An impure pipe is called often, as often as every keystroke or mouse-move.

**_What is slice pipe? _**

The slice pipe is used to create a new Array or String containing a
subset (slice) of the elements from the existing array or string.

{{ value_expression | slice : start [ : end ] }}

For example, you can provide 'hello' list based on a greeting array,

@Component({
selector: 'list-pipe',
template: `<ul> <li *ngFor="let i of greeting |
slice:0:5">{{i}}</li> </ul>`})

export class PipeListComponent {
greeting: string[] = ['h', 'e', 'l', 'l', 'o', 'm','o', 'r', 'n', 'i',
'n', 'g'];}

## Angular Router

**_What is Angular Router?_**

Angular Router is a mechanism in which navigation happens from one view to the next as users perform application tasks.

**_What are the router imports?_**

The Angular Router which represents a particular component view for a
given URL is not part of Angular Core. It is available in library
named @angular/router to import required router components.

import { RouterModule, Routes } from '@angular/router';

**_What is router outlet? _**

The RouterOutlet is a directive from the router library and it acts as a
placeholder that marks the spot in the template where the router
should display the components for that outlet. Router outlet is used like
a component,

<.router-outlet></.router-outlet>

<!-- Routed components go here & ignore Dot-->

**_What are router links?_**

The RouterLink is a directive on the anchor tags give the router control over those elements

Since the navigation paths are fixed, you can
assign string values to router-link directive as below, 3.

<h1>Angular Router</h1><nav>
 <a routerLink="/todosList" >List of todos</a>
 <a routerLink="/completed" >Completed
todos</a></nav>

> ignore Dot
> <.router-outlet></.router-outlet>

**_What are active router links?_**

RouterLinkActive is a directive that toggles css classes for active
RouterLink bindings based on the current RouterState. i.e, the Router
will add CSS classes when this link is active and and remove when the
link is inactive. For example, you can add them to RouterLinks as below

<h1>Angular Router</h1><nav>
 <a routerLink="/todosList" routerLinkActive="active">List of
todos</a>
 <a routerLink="/completed" routerLinkActive="active">Completed
todos</a></nav><router-outlet></router-outlet>

**_What is router state? _**

RouterState is a tree of activated routes.
You can access the current RouterState from
anywhere in the application using the Router service and the routerState property

@Component({templateUrl:'template.html'})

class MyComponent {

constructor(router: Router) {

const state: RouterState = router.routerState;
const root: ActivatedRoute = state.root;
const child = root.firstChild;
const id: Observable<string> = child.params.map(p => p.id);
//...
}}

**_What are router events? _**

During each navigation, the Router emits navigation events through the
Router.events property allowing you to track the lifecycle of the route.

**_What is the purpose of Wildcard route?_**

If the URL doesn't match any predefined routes then it causes the
router to throw an error and crash the app. In this case, you can use
wildcard route.

For example, you can define PageNotFoundComponent for wildcard route as below

{ path: '\*\*', component: PageNotFoundComponent }
