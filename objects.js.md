# Objects


### I had to get the first value of the property name and then brake. 
```js
var obj = { first: 'someVal' };
obj[Object.keys(obj)[0]]; //returns 'someVal'
```
> Get the first object from the an object with two objects insided 
```js
var myCategoryObject = tires[i]['MyCategory'],
    firstCategoryObject = tires[i]['MyCategory'][Object.keys(tires[i]['MyCategory'])];

```

