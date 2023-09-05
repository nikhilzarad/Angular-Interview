# Angular-Interview

 ## Angular Service

 ***What is a service?***

 It is Class which is used to create components or common functionality which can be shared
across the entire application.

***Example***
  
   API call function and normal function which can accept parameter and return a value.

***How to Use Service***

ng generate service my-new-service: add a service to your
application

import { Injectable } from '@angular/core';
import { Http } from '@angular/http';

@Injectable({
 > The Injectable decorator is required for dependency
 injection to work

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


 ## observable & observer

 ***What is an observable?***

 It is provide support for passing messages between publishers and subscribers in your
application.

They are mainly used for event handling,asynchronous programming, and handling multiple values. 
In this case, you Make a function for publishing values, but it is not
executed until a consumer subscribes to it.


***What is subscribing or observer ?***

An Observable instance begins publishing values only when someone
subscribes to it. So you need to subscribe by calling
the subscribe() method of the instance, passing an observer object to
receive the notifications.

***Example***

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
>Observer got a next value: 4
> Observer got a next value: 5
>Observer got a complete notification

