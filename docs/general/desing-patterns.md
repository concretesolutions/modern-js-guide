# Javascript design patterns

The content of this page is originally extracted from http://stackoverflow.com/documentation/javascript/1668/creational-design-patterns#t=201703072019236686439

Design patterns are a good way to keep your code readable and DRY. DRY stands for don't repeat yourself. Below you could find more examples about the most important design patterns.

* [Factory function](#factory-function)
* [Factory with composition](#factory-with-composition)
* [Module and Revealing Module Patterns](#module-and-revealing-module-patterns)


## Factory function
A factory function is simply a function that returns an object.

Factory functions do not require the use of the `new` keyword, but can still be used to initialize an object, like a constructor.

Often, factory functions are used as API wrappers, like in the cases of jQuery and moment.js, so users do not need to use `new`.

The following is the simplest form of factory function; taking arguments and using them to craft a new object with the object literal:

```javascript
function cowFactory(name) {
    return {
        name: name,
        talk: function () {
            console.log('Moo, my name is ' + this.name);
        },
    };
}

var daisy = cowFactory('Daisy');  // create a cow named Daisy
daisy.talk();  // "Moo, my name is Daisy"
```

It is easy to define private properties and methods in a factory, by including them outside of the returned object. This keeps your implementation details encapsulated, so you can only expose the public interface to your object.

```javascript
function cowFactory(name) {
    function formalName() {
        return name + ' the cow';
    }

    return {
        talk: function () {
            console.log('Moo, my name is ' + formalName());
        },
    };
}


var daisy = cowFactory('Daisy');
daisy.talk();  // "Moo, my name is Daisy the cow"
daisy.formalName();  // ERROR: daisy.formalName is not a function
```
The last line will give an error because the function formalName is closed inside the cowFactory function. This is a closure.

Factories are also a great way of applying functional programming practices in JavaScript, because they are functions.

## Factory with composition
'Prefer composition over inheritance' is an important and popular programming principle, used to assign behaviors to objects, as opposed to inheriting many often unneeded behaviors.

### Behaviour factories
```javascript
var speaker = function (state) {
    var noise = state.noise || 'grunt';

    return {
        speak: function () {
            console.log(state.name + ' says ' + noise);
        }
    };
};

var mover = function (state) {
    return {
        moveSlowly: function () {
            console.log(state.name + ' is moving slowly');
        },
        moveQuickly: function () {
            console.log(state.name + ' is moving quickly');
        }
    };
};
```
### Object factories
```javascript
var person = function (name, age) {
    var state = {
        name: name,
        age: age,
        noise: 'Hello'
    };

    return Object.assign(     // Merge our 'behaviour' objects
        {},
        speaker(state),
        mover(state)
    );
};

var rabbit = function (name, colour) {
    var state = {
        name: name,
        colour: colour
    };

    return Object.assign(
        {},
        mover(state)
    );
};
```

### Usage

```javascript
var fred = person('Fred', 42);
fred.speak();        // outputs: Fred says Hello
fred.moveSlowly();   // outputs: Fred is moving slowly

var snowy = rabbit('Snowy', 'white');
snowy.moveSlowly();  // outputs: Snowy is moving slowly
snowy.moveQuickly(); // outputs: Snowy is moving quickly
snowy.speak();       // ERROR: snowy.speak is not a function
```
### More about composition over inheritance

https://medium.com/humans-create-software/factory-functions-in-javascript-video-d38e49802555#.q6x747ihn
http://softwareengineering.stackexchange.com/questions/134097/why-should-i-prefer-composition-over-inheritance

## Module and Revealing Module Patterns

### Module pattern
The Module pattern is a [creational and structural design pattern](https://en.wikipedia.org/wiki/Module_pattern#Module_as_a_design_pattern) which provides a way of encapsulating private members while producing a public API. This is accomplished by creating an [IIFE](http://stackoverflow.com/documentation/javascript/186/functions/843/immediately-invoked-function-expressions) which allows us to define variables only available in its scope (through [closure][1]) while returning an object which contains the public API.

This gives us a clean solution for hiding the main logic and only exposing an interface we wish other parts of our application to use.
