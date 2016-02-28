# Using Array.reduce()
###January 18th 2016

Array reduce is good for immutiable data.  Immutable data is good for large application structures (https://facebook.github.io/immutable-js/).
Persistant data presents a mutative API which does not update the data in-place, but instead yields new updated data. 

There are a few arguments for using immutable data structures in JS.
###Predictability
Mutability hides change which creates (unexpected) side effects, which can cause nasty bugs. 
When you introduce immutability you keep application architecture and mental model simple. 

###Performance 
Immutable Objects can make use of structural sharing to reduce memory overhead.

###Mutation Tracking
immutability allows you to optimize your application by making use of reference and value equality.
This makes it easy to see if anything has changed.  For example a state change in a React component.  
The ```shouldComponentUpate``` checks against to see if the state is identical by comparing state objects.

####To understand immutability, understand reference equality and value equality in JS data structures. 


##Examples of mutable object and immutability with object.assign() and spread 
>Use Object.assign() to create a new object instead of mutating the object. 

>Mutated object.
```js
const toggleTodo = (todos) => {
    todos.completed = !s.completed;
    return todos;
}
```

>Immutatble Object with new object literal.
```js
const toggleTodo = (todos) => {
    return {
        id: todos.id,
        text: todos.text,
        completed: !todos.completed
    };
}

The returned immutable object could present bugs if we add new properties 
```

> Immuatable Objecs with Object.assig( {}, obj, )
```js
/*********************************************************************
* assign properties of several object onto target object. 
* The argument order coresponds to the assign operator

* the left argument is the ones whoes property will be asssigned to
* every further arguments will represent one of the source objects 
* whose properties will be copied to the target object. 
* If serveral sources represent the same properties the last one wins. 
***********************************************************************/
const toggleTodo = (todos) => {
    return Object.assign({}, todos, {
        completed: !todo.completed
    });
}
```
>Alternately you could use the spread operator.
```js
const toggleTodo = (todos) => {
    return {
        ...todos,
        completed: !todo.completed
    };

}
```