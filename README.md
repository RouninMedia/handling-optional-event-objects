# Handling Optional Event Objects
...because it's helpful to be able to re-use the same function both inside and outside of `EventListeners`.

_____

Occasionally, functions which are usually (or sometimes) invoked via `EventListeners` may also be invoked directly.

In the `EventListener`, the first parameter passed to the function will be an `Event object`.

In the second situation, no parameter passed to the function will represent an `Event object`.

There doesn't seem to be a standard way of identifying whether the first parameter is or is not an `Event` object. (Perhaps it's widely regarded an anti-pattern to invoke the same function directly, which is usually invoked by an `EventListener`? But I've not heard this either.)

However. There are several approaches to successfully identifying whether the *first* parameter ***is*** an `Event object` or not.

_____

## Approach #1

The first approach is to query that parameter's `constructor.name` - which, in all cases, will end in `Event`.

**E.g.**

Where the first parameter of a function (usually `e` or `event`) is named `eventObject`:

    let start = (eventObject.constructor.name.length - 5);
    let end = eventObject.constructor.name.length;
    const functionHasEventObject = (eventObject.constructor.name.substring(start, end) === 'Event') ? true : false
    
### Full Example:

This example works whether the function's parameters comprise of:

  1. Two parameters: an `Event object` followed by an `Object`
  2. Two parameters: an `Event object` followed by a `String`
  3. One parameter: an `Object`
  4. One parameter: a `String` 

#### HTML

    <header class="header" data-danis3h-events="{«mouseout:testFunction»: {«data»: {«stringToLog»: «Fantastic - this is working!»}}}">

#### JavaScript

    const testFunction = (eventObject, eventActionData) => {

      let start = (eventObject.constructor.name.length - 5);
      let end = eventObject.constructor.name.length;
      
      eventActionData = (eventObject.constructor.name.substring(start, end) === 'Event') ? eventActionData : eventObject;
  
      let stringToLog = (eventActionData.constructor.name === 'Object') ? eventActionData.stringToLog : eventActionData;
      console.log(stringToLog);
    }

    testFunction({stringToLog: 'This object on its own is working too - without a preceding Event Object parameter.'});
    testFunction('And this string on its own. This works too.');
