# Javascript design patterns

The content of this page is originally extracted from http://stackoverflow.com/documentation/javascript/1668/creational-design-patterns#t=201703072019236686439

Design patterns are a good way to keep your code readable and DRY. DRY stands for don't repeat yourself. Below you could find more examples about the most important design patterns.

* [Factory function](#factory-function)
* 


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
```

var daisy = cowFactory('Daisy');
daisy.talk();  // "Moo, my name is Daisy the cow"
daisy.formalName();  // ERROR: daisy.formalName is not a function
The last line will give an error because the function formalName is closed inside the cowFactory function. This is a closure.

Factories are also a great way of applying functional programming practices in JavaScript, because they are functions.
