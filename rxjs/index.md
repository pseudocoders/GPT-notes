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

### tap

In RxJS, the `tap` operator is a utility operator that allows you to perform side effects or observe the values emitted by an observable stream without modifying the values themselves. It is often used for debugging, logging, or performing actions that don't alter the stream's data.

The signature of `tap` looks like this:

```javascript
tap(
  nextFn?: (value: T) => void,
  errorFn?: (error: any) => void,
  completeFn?: () => void
): Observable<T>
```

- `nextFn`: A function that will be called when a new value is emitted by the observable.

- `errorFn`: A function that will be called if an error occurs in the observable.

- `completeFn`: A function that will be called when the observable completes.

Here's a breakdown of how `tap` is commonly used:

### Logging and Debugging

```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.of(1, 2, 3);
            source.pipe(
                rxjs.tap(value => o.innerHTML += `Before Mapping: ${value} <br>`),
                rxjs.map(value => value * 2),
                rxjs.tap(value => o.innerHTML += `After Mapping: ${value} <br>`)
            ).subscribe(observer);
```

In this example, the `tap` operator is used to log values before and after the `map` operator is applied, providing insight into the data at different stages of the observable pipeline.

### Side Effects

```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.from([1, 2, 3, 4, 5]);
            source.pipe(
                rxjs.tap(value => o.innerHTML += `Checking value: ${value} <br>`),
                rxjs.filter(value => value % 2 === 0),
                rxjs.tap(value => o.innerHTML += `Even value found: ${value} <br>`)
            ).subscribe(observer);
```

Here, `tap` is used to log messages during the process of filtering even numbers. It doesn't affect the stream's values but allows you to observe what's happening.

### Resource Cleanup

```javascript
            rxjs.fromEvent(document, 'click').pipe(
                rxjs.tap(() => o.innerHTML += 'Click detected <br>'),
                rxjs.take(5),
                rxjs.tap(() => o.innerHTML += 'Unsubscribing after 5 clicks <br>')
            ).subscribe({
                next: n => o.innerHTML += "x: " + n.x + " - y: " + n.y + "<br>",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            });
```

In this case, `tap` is used to log messages when a click is detected and when the observable is unsubscribed after a certain number of clicks.

Remember that while `tap` is useful for debugging and performing side effects, it's crucial not to misuse it for transforming data or making asynchronous calls. If you need to transform the data, consider using other operators like `map`, `filter`, etc.

### BASIC OPERATORS

#### map
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
#### filter
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
#### reduce

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
Accumulator can be initialized:
```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };
            const source = rxjs.range(1, 5);
            const process = source.pipe(rxjs.reduce((acc, v) => acc + v, 100));
            const s = process.subscribe(observer);
```
#### scan
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

### CONDITIONAL OPERATORS

#### every
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.interval(500).pipe(rxjs.take(9));
            const result = source.pipe(rxjs.every(x => x < 10));
            result.subscribe(observer);
```

#### sequenceEqual
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source1 = rxjs.interval(1000).pipe(rxjs.take(9));
            const source2 = rxjs.interval(400).pipe(rxjs.take(9));
            const result = source1.pipe(rxjs.sequenceEqual(source2));
            result.subscribe(observer);
```
### FILTERING OPERATORS

#### take

```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.interval(1000).pipe(rxjs.take(9));
            source.subscribe(observer);
```
### takeUntil

```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.interval(500);
            const clicks = rxjs.fromEvent(document, 'click');
            const result = source.pipe(rxjs.takeUntil(clicks));
            result.subscribe(observer);
```
#### skip
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            rxjs.interval(500).pipe(rxjs.skip(5)).subscribe(observer);
```

#### skipUntil
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.interval(500);
            const clicks = rxjs.fromEvent(document, 'click');
            const result = source.pipe(rxjs.skipUntil(clicks));
            result.subscribe(observer);
```

#### find
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            rxjs.interval(500).pipe(rxjs.find((x) => x === 5)).subscribe(observer);
```
#### first
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
#### last
```javascript
            const observer = {
                next: n => o.innerHTML += n + " - ",
                error: err => o.innerHTML += " Error: " + err,
                complete: () => o.innerHTML += '-->Completed!',
            };
            const source = rxjs.interval(1000).pipe(rxjs.take(9));
            const process = source.pipe(rxjs.last());
            process.subscribe(observer);
```

#### partition
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

### COMBINATION OPERATORS

RxJS provides operators to combine multiple Observables. Combining observables is a common operation in reactive programming, and RxJS provides several operators to facilitate this. Combining observables allows you to work with multiple streams of data simultaneously. Here are some common ways to combine observables in RxJS:

1. **`forkJoin`:**
   - `forkJoin` combines multiple observables into a single observable that emits an array of the last values from each input observable when all of them have completed.
   
   ```typescript
   import { forkJoin, of } from 'rxjs';

   const observable1 = of('A');
   const observable2 = of('B');

   forkJoin([observable1, observable2]).subscribe(values => {
     console.log(values); // Output: ['A', 'B']
   });
   ```

2. **`combineLatest`:**
   - `combineLatest` combines the latest values from multiple observables into an array and emits that array whenever any of the source observables emits a new value.
   
   ```typescript
   import { combineLatest, interval } from 'rxjs';

   const observable1 = interval(1000);
   const observable2 = interval(2000);

   combineLatest([observable1, observable2]).subscribe(values => {
     console.log(values);
     // Output: [0, 0] (when interval1 emits 0)
     //         [1, 0] (when interval1 emits 1)
     //         [1, 1] (when interval2 emits 1)
     //         [2, 1] (when interval1 emits 2)
     //         ...
   });
   ```
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




3. **`zip`:**
   - `zip` combines the values of multiple observables in a strict sequence, emitting an array of the nth values from each observable.
   
   ```typescript
   import { zip, of } from 'rxjs';

   const observable1 = of('A', 'B', 'C');
   const observable2 = of(1, 2, 3);

   zip(observable1, observable2).subscribe(values => {
     console.log(values);
     // Output: ['A', 1], ['B', 2], ['C', 3]
   });
   ```
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
4. **`merge`:**
   - `merge` combines multiple observables into a single observable, emitting values from all of them as they arrive. It does not wait for the completion of one observable before starting another.
   
   ```typescript
   import { merge, interval } from 'rxjs';

   const observable1 = interval(1000);
   const observable2 = interval(500);

   merge(observable1, observable2).subscribe(value => {
     console.log(value);
     // Output: 0 (from interval2)
     //         0 (from interval1)
     //         1 (from interval2)
     //         2 (from interval2)
     //         1 (from interval1)
     //         3 (from interval2)
     //         ...
   });
   ```
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
5. **`concat`:**
   - `concat` concatenates multiple observables, emitting values from the first observable until it completes, then emitting values from the second observable, and so on.
   
   ```typescript
   import { concat, of } from 'rxjs';

   const observable1 = of('A', 'B', 'C');
   const observable2 = of(1, 2, 3);

   concat(observable1, observable2).subscribe(value => {
     console.log(value);
     // Output: 'A', 'B', 'C', 1, 2, 3
   });
   ```
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

These operators provide powerful ways to combine, merge, or coordinate multiple observables in various ways, depending on the specific requirements of your application.

6. **`withLatestFrom`:**


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

## 5. **Subject**
In RxJS (Reactive Extensions for JavaScript), a `Subject` is a special type of observable that acts as both an observer and an observable. It allows values to be multicasted to multiple observers, making it a powerful tool for implementing communication between different parts of an application or coordinating complex asynchronous workflows.

Here are the key characteristics and features of a `Subject`:

1. **Multicasting:**
   - A `Subject` is a multicast observable. It can have multiple subscribers, and when a new value is emitted, it is broadcasted to all the subscribers.

2. **Imperative Emission:**
   - Unlike regular observables that are more declarative in nature (you define what should happen when a certain event occurs), a `Subject` allows you to imperatively emit values using its `next()` method.

3. **Both Observer and Observable:**
   - A `Subject` can act as both an observer and an observable. It means you can subscribe to a `Subject` to receive values, and at the same time, you can push values into the `Subject`.

4. **No Initial Value:**
   - A `Subject` does not have an initial value. When a new observer subscribes to a `Subject`, it does not immediately receive a value.

5. **Late Subscription:**
   - If an observer subscribes to a `Subject` after some values have been emitted, it will miss those previously emitted values. It only receives values emitted after the time of subscription.

Here's a simple example:


```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };
            const subject = new rxjs.Subject();
            subject.next('Bart');
            subject.subscribe(observer);
            subject.next('Homer');
            subject.subscribe(observer); 
            subject.next('Lisa');
            subject.complete();
```

Observables and Subjects are both core concepts in RxJS, but they serve different purposes and have distinct characteristics. Here are the key differences between an Observable and a Subject:

1. **Unicast vs. Multicast:**
   - **Observable:** An Observable is unicast by default. Each subscription to an Observable results in an independent execution of the Observable sequence for each subscriber. In other words, each subscriber gets its own stream of values.
   - **Subject:** A Subject, on the other hand, is multicast. It can have multiple subscribers, and when a new value is emitted, it is broadcasted to all subscribers.

2. **Imperative vs. Declarative:**
   - **Observable:** Observables are more declarative in nature. You define what should happen when a certain event or value is emitted, and subscribers react to those emissions.
   - **Subject:** Subjects allow for imperative emissions. You can actively push values into the Subject using methods like `next()`.

3. **Initial Value:**
   - **Observable:** Observables do not have an initial value. Subscribers only start receiving values when the Observable emits.
   - **Subject:** Subjects do not have an initial value either.

4. **Late Subscription:**
   - **Observable:** If you subscribe to an Observable after it has started emitting values, you miss those previously emitted values. Subscribers only receive values emitted after the time of subscription.
   - **Subject:** Late subscribers to a Subject do not receive previously emitted values by default. However, there are specialized types of Subjects, such as BehaviorSubject and ReplaySubject, that handle this differently.

5. **Both Observer and Observable:**
   - **Observable:** An Observable is mainly an observable sequence, and it is used to represent a stream of values over time.
   - **Subject:** A Subject is both an observer and an observable. It can be subscribed to like an observable to receive values, and it can actively push values to its subscribers like an observer.

In summary, while both Observables and Subjects are fundamental to reactive programming with RxJS, Observables are unicast and more declarative, representing streams of values, while Subjects are multicast, allowing for imperative value emissions and serving as both observers and observables. Additionally, Subjects provide features like multicasting and the ability to act as a bridge between non-observable code and observable code.

Subject characteristics:

1. **No Initial Value:**
   - A `Subject` does not have an initial value.
   - When a new observer subscribes to a `Subject`, it does not immediately receive a value.

2. **Late Subscription:**
   - If an observer subscribes to a `Subject` after some values have been emitted, it will miss those previously emitted values.
   - It only emits values to subscribers that are active at the time of the emission.

3. **Plain Multicast:**
   - A `Subject` simply multicasts values to all of its current subscribers without maintaining any memory of the latest emitted value.


## 6. **BehaviorSubject**

In RxJS (Reactive Extensions for JavaScript), a BehaviorSubject is a type of Observable that represents a value that changes over time. It is part of the broader category of Subjects, which are special types of Observables that allow values to be multicasted to multiple Observers.

The key feature of a BehaviorSubject is that it maintains and emits the current value to all its subscribers, and it also keeps track of the latest value. When a new Observer subscribes to a BehaviorSubject, it immediately receives the current value. Subsequent changes to the value will be notified to all subscribers.

Here's a brief breakdown of the key characteristics of a BehaviorSubject:

1. **Initial Value:** A BehaviorSubject is created with an initial value. This initial value is the value that is emitted to any subscriber immediately upon subscription.

2. **Current Value:** Unlike regular Observables that only push values when an event occurs, a BehaviorSubject always holds a current value. Subscribers can retrieve the current value at any time using the `getValue()` method.

3. **Multicasting:** The BehaviorSubject multicasts its value to all subscribers. This means that multiple subscribers can listen to the same BehaviorSubject, and they will all receive the same current and subsequent values.

4. **Stateful:** The BehaviorSubject is stateful because it retains the last emitted value. This can be useful in scenarios where you need to keep track of and react to the latest value.

Here's a simple example in TypeScript:

```javascript
            // Create a BehaviorSubject with an initial value of 0
            const subject = new rxjs.BehaviorSubject(0);

            // Subscribe to the BehaviorSubject
            subject.subscribe(value => {
                o.innerHTML += `Subscriber A: ${value}<br>`;                
            });

            // Emit a new value
            subject.next(1);

            // Subscribe again
            subject.subscribe(value => {
                o.innerHTML += `Subscriber B: ${value}<br>`;                
            });

            // Emit another value
            subject.next(2);

// Output:
// Subscriber A: 0
// Subscriber A: 1
// Subscriber B: 1
// Subscriber A: 2
// Subscriber B: 2
```

In this example, both Subscriber A and Subscriber B receive the same sequence of values, demonstrating the multicasting behavior of the BehaviorSubject.







BehaviorSubject characteristics:

1. **Initial Value:**
   - A `BehaviorSubject` is created with an initial value.
   - When a new observer subscribes to a `BehaviorSubject`, it immediately receives the current value.

2. **Maintains Latest Value:**
   - A `BehaviorSubject` keeps track of the latest emitted value and replays it to new subscribers.
   - Subscribers can also retrieve the current value at any time using the `getValue()` method.

3. **Stateful:**
   - `BehaviorSubject` is stateful as it retains the last emitted value.

## 7. **ReplaySubject**

In RxJS, a `ReplaySubject` is a type of subject that retains and replays a specified number of values to any new subscribers. It is a type of subject that combines the features of both `Subject` and `BehaviorSubject`. Like a regular `Subject`, it can multicast values to multiple subscribers, and like a `BehaviorSubject`, it keeps a buffer of the last N values so that new subscribers can receive those historical values upon subscription.

Here are the key characteristics of a `ReplaySubject`:

1. **Replay Buffer:**
   - A `ReplaySubject` maintains a buffer that stores a specified number of the most recently emitted values. This buffer is used to replay those values to new subscribers.

2. **Multicasting:**
   - Similar to a regular `Subject`, a `ReplaySubject` can have multiple subscribers, and any value emitted is multicast to all subscribers.

3. **Initial Values:**
   - When a new subscriber subscribes to a `ReplaySubject`, it immediately receives the buffered values, up to the specified buffer size. This is different from a regular `Subject`, which doesn't provide any historical values to late subscribers.

4. **Late Subscription:**
   - Even if a subscriber subscribes after values have been emitted, it will still receive the buffered values up to the specified limit.

Here's a simple example in TypeScript:

```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };

            // Create a ReplaySubject with a buffer size of 2
            const replaySubject = new rxjs.ReplaySubject(2);

            // Emit values
            replaySubject.next(1);
            replaySubject.next(2);
            replaySubject.next(3);

            // Subscribe to the ReplaySubject
            replaySubject.subscribe(observer);

// Output:
// Subscriber A: 1   <-- Subscriber A receives the buffered values upon subscription
// Subscriber A: 2
// Subscriber A: 3
```

In this example, the `ReplaySubject` has a buffer size of 2. When the first subscriber subscribes, it immediately receives the two most recently emitted values (1 and 2). If more values were emitted, the buffer would retain the latest values up to the specified limit.

The ability to provide historical values to late subscribers makes `ReplaySubject` useful in scenarios where you want subscribers to have access to recent data, even if they start observing the stream after some values have already been emitted.

## 9. **AsyncSubject**

In RxJS, an `AsyncSubject` is a type of subject that only emits the last value of the observable sequence to its subscribers, but only when the observable completes. If the observable terminates without completing, the `AsyncSubject` won't emit any value.

Here are the key characteristics of an `AsyncSubject`:

1. **Emits Only on Completion:**
   - An `AsyncSubject` will only emit the last value of the observable sequence to its subscribers when the observable completes. If the observable terminates without completing (e.g., due to an error), the `AsyncSubject` won't emit any value.

2. **Late Subscription:**
   - Even if a subscriber subscribes after values have been emitted, it will still receive only the last value upon completion.

3. **Useful for Late Observers:**
   - `AsyncSubject` is particularly useful when you want to provide a result to late observers only when the observable sequence completes. This is often used when you want to wait until the very end to deliver a result, such as in scenarios where you are calculating a final result over time and only want to provide that result when the computation is complete.

Here's a simple example in TypeScript:

```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };
            // Create an AsyncSubject
            const asyncSubject = new rxjs.AsyncSubject();

            // Emit values
            asyncSubject.next(1);
            asyncSubject.next(2);

            // Subscribe to the AsyncSubject
            asyncSubject.subscribe(observer);

            // Emit another value
            asyncSubject.next(3);

            // Complete the AsyncSubject
            asyncSubject.complete();

            // Subscribe again
            asyncSubject.subscribe(observer);
```

In this example, only the last value (3) is emitted to subscribers when the `AsyncSubject` completes. Both Subscriber A and Subscriber B receive the same value even though Subscriber B subscribes after the value has been emitted.

`AsyncSubject` is a specialized subject and is typically used in scenarios where you want to provide a result to observers only when the observable sequence completes, making it useful for asynchronous computations that yield a final result.

## 8. **Schedulers:**
Schedulers control the execution context of Observables. RxJS provides schedulers to specify when and where the Observables should be observed. Commonly used schedulers include `observeOn` and `subscribeOn`.

In RxJS, a scheduler is an abstraction that manages the execution of asynchronous operations. It provides a way to control when and where the work associated with an observable is performed. Schedulers are particularly useful for handling concurrency, timing, and controlling the execution context.

Here are the key concepts related to RxJS schedulers:

1. **Concurrency Control:**
   - Schedulers help control the concurrency of operations in an observable sequence. They determine when and on which execution context (e.g., synchronous or asynchronous) the observable's operations are performed.

2. **Execution Context:**
   - Schedulers provide an abstraction for the execution context. This could be the current JavaScript execution context (synchronous), a microtask queue (asynchronous), or a specific thread in environments that support multi-threading.

3. **Types of Schedulers:**
   - **ImmediateScheduler:** Represents the current synchronous execution context. It executes tasks immediately.
   - **AsapScheduler:** Represents the microtask queue, ensuring that tasks are executed after the current execution context and before the next browser repaint.
   - **QueueScheduler:** Represents a generic queue for scheduling tasks. It is often used for asynchronous operations.
   - **AnimationFrameScheduler:** Schedules tasks to be executed in the next browser animation frame.

4. **Custom Schedulers:**
   - RxJS allows you to create custom schedulers to suit specific application requirements. This can be useful when integrating with external systems or frameworks that have their own scheduling mechanisms.

Here's a simple example using the `observeOn` operator, which allows you to specify a scheduler for an observable:

```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };

            // Create an observable
            const observable = rxjs.of(1, 2, 3);

            // Use observeOn to specify a scheduler (e.g., asyncScheduler)
            const scheduledObservable = observable.pipe(rxjs.observeOn(rxjs.asyncScheduler));

            // Subscribe to the scheduled observable
            scheduledObservable.subscribe(observer);
```

In this example, the `observeOn` operator is used to specify that the observable should be observed on the `asyncScheduler`. This means that the subscription callback will be scheduled to run asynchronously using the microtask queue.

Schedulers are a powerful tool in RxJS for managing the timing and concurrency of observable sequences, and they provide a flexible way to integrate with different execution contexts. They help make RxJS adaptable to various environments, whether synchronous or asynchronous.

```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };

            const source = rxjs.of('Hello', 'World').pipe(
                rxjs.observeOn(rxjs.asyncScheduler)
            );

            source.subscribe(observer);
```

## 7. **Error Handling:**
RxJS provides operators for handling errors in Observables. The `catchError` operator is commonly used to gracefully handle errors and continue the Observable stream.

```javascript
            const observer = {
                next: value => o.innerHTML += value + '<br>',
                error: error => o.innerHTML += "ERROR: " + error + '<br>',
                complete: () => o.innerHTML += 'Completed' + '<br>',
            };

            const source = rxjs.of('Hello', 'World').pipe(
                rxjs.map(() => {
                    throw new Error('Custom Error');
                }),
                rxjs.catchError(error => rxjs.of(`Caught an error: ${error.message}`))
            );

            source.subscribe(observer);
            const source2 = rxjs.of('Hello', 'World').pipe(
                rxjs.map(() => {
                    throw new Error('Custom Error');
                }),
                rxjs.catchError(error => oof(`Caught an error: ${error.message}`))
            );

            source2.subscribe(observer);
```



