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

ReactiveX (Rx) is a library for composing asynchronous and event-based programs using observable sequences. RxJS is the JavaScript implementation of ReactiveX, and it is widely used in frontend and backend development, particularly with frameworks like Angular.

Here are some key concepts and features of RxJS:

1. **Observables:**
   - At the core of RxJS is the concept of observables. An observable represents a stream of values or events that can be observed. These values can be emitted over time, and observers can subscribe to the observable to react to these emissions.

2. **Observers:**
   - Observers are the entities that subscribe to observables to receive notifications when values are emitted. An observer typically consists of three callback functions: `next` (handles each emitted value), `error` (handles errors), and `complete` (indicates the completion of the observable).

3. **Operators:**
   - RxJS provides a rich set of operators that allow you to transform, filter, combine, and manipulate observables. These operators make it easy to compose complex asynchronous logic in a declarative and readable manner.

4. **Subscription:**
   - When an observer subscribes to an observable, it creates a subscription. Subscriptions can be used to manage the lifecycle of the observation. You can unsubscribe to stop receiving notifications and prevent memory leaks.

5. **Subjects:**
   - Subjects are both observables and observers. They allow values to be multicasted to many observers, making it possible to broadcast values to multiple parts of an application.

6. **Schedulers:**
   - Schedulers in RxJS provide a way to control the concurrency of observables. They enable you to specify when and where the code associated with an observable should run, helping to manage asynchronous operations.

7. **Error Handling:**
   - RxJS provides mechanisms for handling errors in observables, allowing you to gracefully handle and recover from errors that may occur during the lifecycle of an observable.

8. **Hot and Cold Observables:**
   - Observables can be categorized as hot or cold. Cold observables start producing values when a subscription is made, while hot observables produce values regardless of subscriptions. Subjects are an example of hot observables.

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
            o.innerHTML += 'RXJS Examples<br>';
        });
    </script>

</body>
</html>           
```
