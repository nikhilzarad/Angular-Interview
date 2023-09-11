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

***What is dependency injection in Angular? ***

It is a class asks for dependencies from external
sources rather than creating them itself.

Dependency Injection is nothing but process of object creation into constructor of any class.

Means We create object of HttpClient into constructor as below is called Dependency Injection .

***Example***

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


***What is the difference between promise and observable? ***

**Observable** :Declarative: An Observable instance begins publishing values 
only when someone subscribes to it.
**Promise**:Execute immediately on creation

**Observable**: Provide multiple values over time
**Promise**:Provide only one

**Observable**: Subscribe method is used for error handling
**Promise**:Push errors to the child promises


***How do you perform error handling in observables?***

You can handle errors by define an error callback on the observer
instead of try/catch Method

**Example**

myObservable.subscribe({

 next(num) { console.log('Next num: ' + num)},

 error(err) { console.log('Received an errror: ' + err)}});


## RxJS

***What is RxJS? ***r

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


***What are the utility functions provided by RxJS?***

i. Converting existing code for async operations into observables
ii. Iterating through the values in a stream
iii.Mapping values to different types
iv. Filtering streams


***what is an rxjs subject in Angular***

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

***What is HttpClient and its benefits?***

Most of the Front-end applications communicate with backend services
over HTTP protocol using either XMLHttpRequest interface or the fetch() API.

import { HttpClientModule } from '@angular/common/http';


***Explain on how to use HttpClient with an example?***


> Import HttpClient into root module:

import { HttpClientModule } from '@angular/common/http';

@NgModule({
 imports: [
 BrowserModule,
 > import HttpClientModule after BrowserModule.
 HttpClientModule,
 ],
 ......
 })

export class AppModule {}

> Inject the HttpClient into the application: Let's
create a userProfileService(userprofile.service.ts) as an example. It also defines get method of
HttpClient

import { Injectable } from '@angular/core';import { HttpClient }
from '@angular/common/http';

const userProfileUrl: string = 'assets/data/profile.json';

@Injectable()export class UserProfileService {


 constructor(private http: HttpClient) { }


 getUserProfile() {
 return this.http.get(this.userProfileUrl);
 }}


> Create a component for subscribing service: Let's
create a component called UserProfileComponent(userprofile.component.ts) which inject UserProfileService and invokes the service method,


fetchUserProfile() {
 this.userProfileService.getUserProfile()
 .subscribe((data: User) => this.user = {
 id: data['userId'],
 name: data['firstName'],
 city: data['city']
 });}


> Since the above service method returns an Observable which needs to be subscribed in the component.












