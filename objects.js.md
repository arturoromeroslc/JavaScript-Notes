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

if($.isEmptyObject(myCategoryObject)) {
    return 'Data Not Available';
}else{
    return firstCategoryObject.name.toUpperCase();
}
```
> A better way:
```js
var myCategoryObject = tires[i]['MyCategory'],
    firstProperty,
    firstCategoryObject;

for (var prop in myCategoryObject) {
    firstProperty = prop;
    break;
}
// $.each(myCategoryObject, function(key, value){
//     firstProperty = key;
//     return false;
// })

var firstCategoryObject = myCategoryObject[firstProperty];

if($.isEmptyObject(myCategoryObject)) {
    return 'Data Not Available';
}else{
    return firstCategoryObject.name.toUpperCase();
}
```
```js
html: function() {
    console.log(tires[i])
    if(window.location.hash) {
        var categoryURL = window.location.hash.split('/')
        for(var i = 0; i < categoryURL.length; i++) {
            if(categoryURL[i].substring(0, 8) === "Category"){
                categoryURL = parseInt(categoryURL[i].replace('Category:','').replace('~','').split(''));
                break;
            }
        }
        console.log(categoryURL);
    }    
    console.log(tires[i]);
    var myCategoryObject = tires[i]['MyCategory'],
        firstProperty,
        firstCategoryObject;

    for (var prop in myCategoryObject) {
        firstProperty = prop;
        break;
    }

    var firstCategoryObject = myCategoryObject[firstProperty];

    if($.isEmptyObject(myCategoryObject)) {
        return "<?= htmlentities(__("Not Applicable", true), ENT_COMPAT, "UTF-8");?>";
    }else{
        return firstCategoryObject.name.toUpperCase();
    }
}
```
```js
if (called == 'undefined') {
    console.log('called: ',called)
    var latlng = new google.maps.LatLng(19.719271, -155.192825);
    console.log(latlng)
} else {
    var latlng = new google.maps.LatLng(19.719271, -155.192825);
    console.log(latlng)
}
```


