# RxJS

ReactiveX (Rx) has its roots in Microsoft's Reactive Extensions (Rx) project, which was initially developed by Erik Meijer and his team at Microsoft. The goal of Reactive Extensions was to provide a unified programming model for handling asynchronous and event-based programming across different platforms and languages.

Here is a brief timeline of RxJS's inception and evolution:

1. **Reactive Extensions (Rx) in .NET (2009):**
   - The Reactive Extensions for .NET (Rx.NET) was officially released by Microsoft in 2009. It introduced the concept of observables and provided a set of operators for composing asynchronous and event-based programs.

2. **Expansion to Other Platforms (2011-2013):**
   - Building on the success of Rx.NET, the concept of Reactive Extensions was extended to other platforms and languages, including JavaScript, Java, Scala, and others. This expansion led to the creation of ReactiveX, a cross-platform initiative for reactive programming.

3. **RxJS for JavaScript (2011):**
   - RxJS, the JavaScript implementation of ReactiveX, was released as an open-source project in 2011. It quickly gained popularity in the JavaScript community for its ability to simplify the management of asynchronous code and handle complex event-driven scenarios.

4. **Angular and RxJS (2013-2014):**
   - RxJS gained even more prominence with the release of Angular 2 in 2016. Angular embraced reactive programming, and RxJS became an integral part of Angular's architecture. Many features in Angular, including HTTP requests and forms, are built on top of RxJS observables.

5. **RxJS 5 (2016):**
   - In 2016, RxJS underwent a major overhaul with the release of version 5. The library was rewritten in TypeScript, providing better typings and improved performance. The API remained largely the same, but the internals were significantly revamped.

6. **RxJS 6 (2018):**
   - RxJS 6, released in 2018, introduced some breaking changes and improvements. One notable change was the introduction of lettable operators, allowing for a more modular and tree-shakable library. This version aimed to reduce the size of the library when used in modern JavaScript applications.

7. **RxJS 7 and Continuous Evolution:**
   - As of my knowledge cutoff in January 2022, RxJS 7 had not been released. However, the RxJS project follows semantic versioning, and new versions are periodically released with improvements, bug fixes, and sometimes new features. Developers are encouraged to stay updated with the latest releases and documentation.

RxJS continues to be a crucial library in the JavaScript ecosystem, providing developers with powerful tools for managing asynchronous and event-driven code. Its evolution reflects the changing landscape of JavaScript development and the ongoing effort to provide more efficient and user-friendly solutions for reactive programming.

## Basics of Reactive Programming

Reactive Programming is a programming paradigm that deals with asynchronous data streams and the propagation of changes. It provides a declarative way to express the relationship between different pieces of data and the transformations applied to them over time. Here are the fundamental concepts of reactive programming:

1. **Event/Stream:**
   - In reactive programming, the basic building block is the event or stream. It represents a sequence of ongoing events ordered in time.
   - Events can be user inputs, sensor outputs, or any occurrence that your program needs to react to.

2. **Observer:**
   - The observer is an object that expresses an interest in certain events or values.
   - It subscribes to an observable to receive notifications whenever the observable emits a new value or event.

3. **Observable:**
   - An observable is a representation of a stream that can be observed. It emits values over time.
   - Observables can be created from various sources, including existing data structures, events, or through the use of specific operators.

4. **Subscription:**
   - The connection between an observer and an observable is established through a subscription.
   - When an observer subscribes to an observable, it receives notifications until the subscription is either unsubscribed or completed.

5. **Operators:**
   - Operators are functions that transform, filter, or combine observables to produce new observables.
   - Common operators include `map`, `filter`, `merge`, `concat`, `combineLatest`, and many more.

6. **Transformations:**
   - Reactive programming allows you to transform data streams over time.
   - Transformations can include filtering out unwanted data, mapping values, combining multiple streams, and more.

7. **Map and FlatMap (MergeMap):**
   - `map` transforms each item emitted by an observable.
   - `flatMap` (or `mergeMap`) not only transforms the items but also flattens the result, handling nested observables.

8. **Filtering:**
   - Filtering operators such as `filter` allow you to selectively allow or disallow items emitted by an observable.

9. **Combining:**
   - Combining operators like `combineLatest`, `zip`, or `merge` enable the merging or coordination of multiple observables.

10. **Error Handling:**
   - Reactive programming provides mechanisms for handling errors in a stream, ensuring that failures are gracefully managed.

11. **Hot and Cold Observables:**
   - Cold observables start emitting items when a subscription is made, and each subscriber gets its own independent sequence of values.
   - Hot observables emit items regardless of subscribers and subscribers receive the items emitted after they subscribe.

12. **Backpressure:**
   - Backpressure is a mechanism for handling scenarios where the rate of events emitted by an observable is faster than the rate at which the observer can consume them.

13. **Schedulers:**
   - Schedulers are used to control the execution context in which the observable emits items and the observer consumes them.

14. **Reactive Extensions (Rx):**
   - Reactive Extensions (often denoted as Rx) is a set of libraries for different programming languages that implement reactive programming patterns.

Understanding these fundamental concepts is crucial for working effectively with reactive programming libraries like RxJS. They provide the foundation for creating responsive, scalable, and maintainable applications, especially in scenarios involving complex asynchronous data flows.

ReactiveX (Rx) is a library for composing asynchronous and event-based programs using observable sequences. RxJS is the JavaScript implementation of ReactiveX, and it is widely used in frontend and backend development, particularly with frameworks like Angular.

RxJS is commonly used in scenarios where there is a need for handling asynchronous operations, such as user interactions, HTTP requests, and other event-based systems. It has become particularly popular in the context of Angular applications, where it is often used for managing state, handling asynchronous data, and event handling.

## Template for examples

```javascript
<!DOCTYPE html>
<html>
<head>
    <!-- https://cdnjs.com/libraries/rxjs -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/rxjs/7.8.1/rxjs.umd.min.js"
        integrity="sha512-D9LDs8YUUVa4V9Gl4Zb+xqRAc7RCzooR3+zzebgK2RMu/KU+dh90pbjEEMzPiSyRSGbSp9j1pZnrO4joGa5WEg=="
        crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    <!-- https://cdnjs.com/libraries/axios -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/1.6.2/axios.min.js"
        integrity="sha512-b94Z6431JyXY14iSXwgzeZurHHRNkLt9d6bAHt7BZT38eqV+GyngIi/tVye4jBKPYQ2lBdRs0glww4fmpuLRwA=="
        crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width">
    <title>RxJS</title>
</head>

<body>
    <h1>RxJS</h1>
    <pre id="output"></pre>
     <script>
        window.addEventListener('DOMContentLoaded', () => {
            var o = document.getElementById("output");
            o.innerHTML += '<h2>RXJS Examples</h2>';
        });
    </script>

</body>
</html>           
```






```javascript
```


RxJS, or Reactive Extensions for JavaScript, is a library for reactive programming using Observables. Observables are a crucial part of RxJS, but there are several other core concepts to understand. Let's delve into the key elements of RxJS:

## 1. Observable, Observer and Subscription
* An Observable represents a stream of data or events that can be observed over time. It can emit multiple values asynchronously. Observables can be created from various sources, including events, arrays, promises, and more.
* An Observer is an object with three callback functions: `next`, `error`, and `complete`. It subscribes to an Observable to receive notifications when the Observable emits values. The `next` function is called when a new value is emitted, `error` handles errors, and `complete` is called when the Observable is done emitting values.
* A Subscription represents the execution of an Observable. It is created when you subscribe to an Observable and can be used to unsubscribe, releasing resources and stopping the Observable from emitting further values.

### Observable generation functions
#### new 
```javascript
            const data = new rxjs.Observable(observer => {
                const origin = [
                    { id: 1, name: 'A' },
                    { id: 2, name: 'B' },
                    { id: 3, name: 'C' },
                ];
                for (let x of origin) {
                    observer.next(x);
                }
                observer.complete();
            });

            data.subscribe(observer => {
                o.innerHTML += `<p>${observer.id} - ${observer.name}</p>`;
            });
```

#### create

```javascript
            const observable01 = rxjs.Observable.create(observer01 => {
                observer01.next("hello");
                observer01.next("mundo");
                observer01.complete();
            });

            observable01.subscribe(
                next => o.innerHTML += next + "<br>",
                err => o.innerHTML += " Error: " + err + "<br>",
                () => o.innerHTML += '--> Completed!' + "<br>",
            );
```

```javascript
            const observable = rxjs.Observable.create(observer => {
                observer.next("how");
                observer.next("are");
                observer.next("you?");
                throw "cacheable error";
                observer.next("end!");
            });

            observable.subscribe(
                next => o.innerHTML += "next:" + next + "<br>",
                err => o.innerHTML += "error:" + err + "<br>",
                () => o.innerHTML += "the end <br>"
            );
```

```javascript
            const observable01 = rxjs.Observable.create(observer01 => {
                observer01.next("hello");
                observer01.next("mundo");
                observer01.complete();
            });

            const observer02 = {
                next: value => o.innerHTML += value + ' from observer02' + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };

            const observer03 = {
                next: value => o.innerHTML += value + ' from observer03' + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };

            const subscription1 = observable01.subscribe(observer02);
            const subscription2 = observable01.subscribe(observer03);
            subscription1.unsubscribe();
            subscription2.unsubscribe();
```

```javascript
            const observable01 = rxjs.interval(1000);

            const observer02 = {
                next: value => o.innerHTML += value + ' from observer02' + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };

            const observer03 = {
                next: value => o.innerHTML += value + ' from observer03' + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };

            const subscription1 = observable01.subscribe(observer02);
            const subscription2 = observable01.subscribe(observer03);
            
            setTimeout (() => {
                subscription1.unsubscribe();
            }, 10000);

            setTimeout (() => {
                subscription2.unsubscribe();
            }, 15000);
```


#### from

```javascript
            // forma deprecada
            var source = rxjs.from([1, 2, 3, 4, 5]);
            var subscription = source.subscribe(
                function (x) {
                    o.innerHTML += x;
                },
                function (err) {
                    o.innerHTML += ' Error: ' + err;
                },
                function () {
                    o.innerHTML += '--> Completed!';
                });        
```

```javascript
            var source = rxjs.from([1, 2, 3, 4, 5]);
            source.subscribe(
                next => o.innerHTML += next,
                err => o.innerHTML += " Error: " + err,
                () => o.innerHTML += '-->Completed!',
            );
```
```javascript
            rxjs.from("hello").subscribe(x => o.innerHTML += x);
```
#### of
```javascript
            rxjs.from([1,2,3,4,5,6]).subscribe(x => o.innerHTML += x + '<br>');
            rxjs.of([1,2,3,4,5,6]).subscribe(x => o.innerHTML += x + '<br>');
```
#### range
```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };
            const nums = rxjs.range(1, 10);
            const subscription = nums.subscribe(observer);
```
#### fromEvent
```javascript
            const observer03 = {
                next: n => o.innerHTML += "(x: " + n.x + " - y: " + n.y + ")" + "<br>",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Done!',
            };
            const clicks = rxjs.fromEvent(document, 'click')
            clicks.subscribe(observer03);
```
#### interval
```javascript
            const observable = rxjs.interval(1000)
            observable.subscribe(i => o.innerHTML += `<br>observable: ${i}`)
```
#### timer
```javascript
            const timer = rxjs.timer(2000)
            timer.subscribe(done => o.innerHTML += '<p>Timer done</p>');
```
```javascript
            const timer = rxjs.timer(1000, 2000)
            timer.subscribe(done => o.innerHTML += '<p>Timer done</p>');
```
#### ajax
OJO ---> ERROR!
```javascript
            const obs = rxjs.ajax('https://jsonplaceholder.typicode.com/posts');
            obs2.subscribe(data => o.innerHTML += `<p>${data.id}</p>`);
```
Usando axios
```javascript
            const observer = {
                next: n => o.innerHTML += n.data.title + "<br>",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!<br>',
            };
            const responsePromise = axios.get('https://jsonplaceholder.typicode.com/todos/1');
            const response = rxjs.from(responsePromise);
            response.subscribe(observer);
```

#### empty
Returns an observable that immediately completes.

## 4. **Operators:**
Operators are functions that transform, filter, or combine Observables. RxJS provides a rich set of operators to manipulate data streams. 

Common operators include `map`, `filter`, `mergeMap`, `switchMap`, `combineLatest`, and more.

### pipe

In RxJS, the `pipe` function is a powerful and flexible tool for composing operations on observables. It allows you to chain multiple RxJS operators together in a more readable and maintainable way.

The `pipe` function takes one or more operators as arguments and returns a new observable with those operators applied in sequence. This is a more functional and composable way to work with observables compared to chaining operators directly on an observable instance.

Here's a basic example to illustrate how `pipe` works:

```javascript
           const source = rxjs.of(1, 2, 3, 4, 5);

            const modifiedObservable = source.pipe(
                rxjs.filter(value => value % 2 === 0), // Keep only even numbers
                rxjs.map(value => value * 2) // Double each remaining number
            );

            modifiedObservable.subscribe(result => o.innerHTML += '<br>' + result);
```

In this example, the `pipe` function is used to compose two operators (`filter` and `map`) on the `source` observable. It filters out odd numbers and then doubles each remaining number. The result is a new observable, `modifiedObservable`, which emits the modified values.

Key Points about `pipe`:

1. **Readability and Composition:**
   - `pipe` makes it easier to read and understand the sequence of operations applied to an observable.
   - You can chain multiple operators without nesting them, enhancing code readability.

2. **Immutability:**
   - Observables are immutable, and the `pipe` function returns a new observable with the applied operators. The original observable remains unchanged.

3. **Separation of Concerns:**
   - `pipe` promotes a separation of concerns by allowing you to define and reuse individual operators separately from the observable they are applied to.

4. **Reusability:**
   - You can create reusable operator functions and easily compose them with `pipe` to create complex data transformations.

5. **Order of Execution:**
   - Operators are applied in the order they appear in the `pipe` function, from left to right.

Here's an example of creating a reusable operator function and using it with `pipe`:

```javascript
            // Reusable operator function
            const double = rxjs.map(value => value * 2);
            const add10 = rxjs.map(value => value + 10);

            const source = rxjs.of(1, 2, 3, 4, 5);

            const modifiedObservable = source.pipe(
                double,
                add10
            );

            modifiedObservable.subscribe(result => o.innerHTML += '<br>' + result);
```

In this example, the `double` operator is created as a reusable function and then used within the `pipe` function along with another operator. This illustrates the reusability and composability of the `pipe` function in creating observable pipelines.

### map
```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };
            const source = rxjs.range(1, 5);
            const process = source.pipe(rxjs.map(x => x * 2));
            const s = process.subscribe(observer);
```
### filter
```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };
            const source = rxjs.range(1, 5);
            const process = source.pipe(rxjs.filter(x => x <= 2));
            const s = process.subscribe(observer);
```
```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };
            const source = rxjs.Observable.create(observer => {
                observer.next("Hola");
                observer.next("Mundo");
                observer.next("sdfd");
                observer.next("rafa");
                observer.next("es el mejor");
                observer.complete();
            });
            const process = source.pipe(rxjs.filter(x => x.length > 4));
            const s = process.subscribe(observer);
```
### reduce

```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };
            const source = rxjs.range(1, 5);
            const process = source.pipe(rxjs.reduce((acc, v) => acc * v));
            const s = process.subscribe(observer);
```
### scan
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.range(1, 5);
            const process = source.pipe(rxjs.scan((acc, x) => acc + x));
            process.subscribe(observer);
```

* scan vs reduce ...

```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };            
            const source = rxjs.interval(500);
            const process = source.pipe(
                rxjs.take(5),
                rxjs.reduce((acc, v) => acc + v)
            );
            const s = process.subscribe(observer);
        
            const process2 = source.pipe(                
                rxjs.scan((acc, v) => acc + v)
            );
            const s2 = process2.subscribe(observer);
```
### concat

```javascript
            var source1 = rxjs.range(1, 3);
            var source2 = rxjs.range(1, 3);
            rxjs.concat(source1, source2).subscribe(result => o.innerHTML += '<br>' + result);
```
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source1 = rxjs.interval(1000);
            const source2 = rxjs.interval(2000);
            const process = rxjs.concat(source1,source2);
            process.subscribe(observer);
```

### merge

```javascript
            var source = rxjs.merge(
                rxjs.range(1, 3),
                rxjs.range(4, 5)
            ).subscribe(result => o.innerHTML += '<br>' + result);
```
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source1 = rxjs.interval(1000);
            const source2 = rxjs.interval(2000);                      
            const process1 = rxjs.merge(source1,source2);            
            process1.subscribe(observer);
```
### zip
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source1 = rxjs.interval(1000);
            const source2 = rxjs.interval(2000);
            const process = rxjs.zip(source1,source2);
            process.subscribe(observer);
```
### combineLatest
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source1 = rxjs.interval(1000);
            const source2 = rxjs.interval(2000);
            const process = rxjs.combineLatest(source1,source2);
            process.subscribe(observer);
```
### withLatestFrom
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source1 = rxjs.interval(1000);
            const source2 = rxjs.interval(2000);
            const process = source1.pipe(rxjs.withLatestFrom(source2));
            process.subscribe(observer);
```

### partition
```javascript
            const observerEven = {
                next: n => o.innerHTML += "even: " + n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const observerOdd = {
                next: n => o.innerHTML += "odd: " + n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.interval(1000);
            
            const [even, odd] = rxjs.partition(source, (value, index) => value % 2 === 0);            
            even.subscribe(observerEven);
            odd.subscribe(observerOdd);
```
### share
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.interval(1000).pipe(rxjs.take(3));
            const process = source.pipe(rxjs.share());
            process.subscribe(observer);
```
### first
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.interval(1000).pipe(rxjs.take(9));
            const process = source.pipe(rxjs.first());
            process.subscribe(observer);
```


### compose operators
```javascript
            rxjs.range(1, 5)
                .pipe(
                    rxjs.filter(x => x % 2 === 1),
                    rxjs.map(x => x + x)
                )
                .subscribe(x => o.innerHTML += (x + " - "));
```




```javascript
```

## 5. **Subjects:**
Subjects are both Observables and Observers. They allow values to be multicasted to many Observers, making them useful for sharing data between parts of your application.

```javascript
import { Subject } from 'rxjs';

const subject = new Subject();

subject.subscribe(observer); // Observer 1
subject.next('Hello');

subject.subscribe(observer); // Observer 2
subject.next('World');
```

## 6. **Schedulers:**
Schedulers control the execution context of Observables. RxJS provides schedulers to specify when and where the Observables should be observed. Commonly used schedulers include `observeOn` and `subscribeOn`.

```javascript
import { of, asyncScheduler } from 'rxjs';
import { observeOn } from 'rxjs/operators';

const source = of('Hello', 'World').pipe(
  observeOn(asyncScheduler)
);

source.subscribe(observer);
```

## 7. **Error Handling:**
RxJS provides operators for handling errors in Observables. The `catchError` operator is commonly used to gracefully handle errors and continue the Observable stream.

```javascript
import { of } from 'rxjs';
import { catchError } from 'rxjs/operators';

const source = of('Hello', 'World').pipe(
  map(() => {
    throw new Error('Custom Error');
  }),
  catchError(error => of(`Caught an error: ${error.message}`))
);

source.subscribe(observer);
```

## 8. **Combining Observables:**
RxJS provides operators to combine multiple Observables. `merge`, `concat`, `combineLatest`, and others are used to handle scenarios where you need to work with multiple streams of data.

```javascript
import { interval, combineLatest } from 'rxjs';

const source1 = interval(1000);
const source2 = interval(500);

combineLatest(source1, source2).subscribe(observer);
```


