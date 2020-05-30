Keywords in RxJs : Observable, Observer, Subscription
Data stream Vs Action Stream
RxJs : is library to read each item in stream and process each of it (collect, process, filter … apples stream example))
Instead of RxJs: 
o	Callback
o	Promise
o	Async – await ( non-cancel, single operation)
 RsJs in angular : in routing, reactive forms (value changes), http client
Code : https://github.com/DeborahK/Angular-RxJs

Course Outline:
•	RxJs Terms & operators
•	Reactive
•	Mapping returned data
•	Combining streams
•	Reacting to actions
•	Caching Observables
•	Higher-Order mapping operators
•	Combining all the streams

RxJs Terms:
 
Observer: 
It is used to observe our stream.
It is collection of callbacks. It is like person being notified about next apple, error apple, completion state.
	Next() & error() & complete()
Subscriber: 
same as Observer but additional method is available: unsubscribe()
Const observer = {
	next: apple => console.log(`apple was emitted ${apple}`),
	error:err => console.log(`Error occured ${err }`),
	complete: () => console.log(`No more apples`)
};
New Observable()
 
Subscribe (): This is used to start the stream of observables.
Subscribe the stream
 
Observer: It is set of callbacks to observe handling next, error and complete
Observable: it is sequence of emitted items
Note: Stream won’t start until we subscribe 
 
List when complete is called in various scenarios:
 
Note: In error and unsubscribe case, Complete methods doesn’t get called.

Creating Observables:
By using creation functions Use of, from, interval, fromEvent:  
	Const appleStream = of(‘apple1’, ‘apple2’);
	Const appleStream = from([‘apple1’, ‘apple2’]);
From is used to create observable from any data structure, 
of can able used like from if we use spread operator as shown below.
 
fromEvent: to create observable from click events on DOM
interval: it also creates observable which emits at defined time 
	const  num = interval(1000).subscribe(console.log);
Note: of & from are static functions they doesn’t need be called like .of or .from
Summary:
 
 

Operators: 
observable -> pipe -> subscribe
 
https://rxjs.dev/
operators: https://rxjs.dev/api
operator decision tree: https://rxjs.dev/operator-decision-tree
If there are multiple operators, the output of first operator will be the input for the next operator:
 
Tap is utility operator
Take emits specified number of items, it is filtering operator
When error occurs, stream stops as shown below and next method is not called.
 

Going Reactive:
Npm install
Npm start




Observable & Subscribe:
Creating Observable in Service and Pipe it
 
Subscribing in component
 
In Component, we can do reactive programming. Declare variables as Observable type e.g products$
e.g.	products$ : Observable<Product[]>; 
•	it implies, products is a observable variable and is not a simple variable in component.
•	Its type is array of products i.e. Product[] where Product is a model with properties.
•	In template, pipe async is used on products$ observable to extract values
 
https://stackblitz.com/edit/angular-gq28uh
Error handling
Two ways : Catch + replace or Catch and replace
use catchError operator is error handling operator, it takes function as argument.
Catch and replace will replace errored observable with new observable.
	 
If any error occurs, we can log that error using catchError operator and again create new Observable as shown above.
In observer, the next method gets called instead of error method as we are creating new observable and returning it.
Catch and throw:
Use throwError creation function to create new observable and 
 
This is used to bubble-up error.
Its return value is Observable<never> as it emits no value and never completes. 
Return empty if we don’t want to return anything.
 
e.g 
  getProducts(): Observable<Product[]> {
    return this.http.get<Product[]>(this.productsUrl)
      .pipe(
        tap(data => console.log('Products: ', JSON.stringify(data))),
        catchError(this.handleError)
      );
  }

private handleError(err: any) {
    // we may send the server to some remote logging infrastructure
    // instead of just logging it to the console
    let errorMessage: string;
    if (err.error instanceof ErrorEvent) {
      // A client-side or network error occurred. Handle it accordingly.
      errorMessage = `An error occurred: ${err.error.message}`;
    } else {
      // The backend returned an unsuccessful response code.
      // The response body may contain clues as to what went wrong,
      errorMessage = `Backend returned code ${err.status}: ${err.body.error}`;
    }
    console.error(err);
    return throwError(errorMessage);
  }

Async pipe: it doesn’t need subscription and unsubscription. It improves change detection.
Performance improvement:
Two options: 
•	Default: this is default, every component is checked when any change is detected
•	OnPush: component is checked only when:
o	@Input properties change
o	Event emits
o	A bound observable emits.
 

