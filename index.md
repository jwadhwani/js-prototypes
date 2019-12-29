# Bob and Javascript Prototypes

_A rose by any other name would smell as sweet - William Shakespeare's Romeo and Juliet_

The subject of Prototypes in Javascript has always been confusing for developers. In this post I will attempt to explain the essence of a prototype in its most basic sense using a fictional developer named Bob.

Bob, a new Javascript developer wanted to build a utility object which contained a few choice _arithmetic_ operations. Along the way he also wanted to get a basic understanding of _prototypes_. Like his fellow developers he looked for some sample code to build on, preferably an object which he could use as a helper or a prototype object to start with. A "hello world" object if you will. Scouring the internet, he found a simple _generic object_, called _utilsObj_ which had a few methods that implemented basic arithmetic operations. 

This was simple enough to analyze and understand. An object with a few methods baked in. Perfect. 

```
//utility object - a prototype or template to build on
const utilsObj = Object.create(null); //empty object
// utility methods
utilsObj.add = function(n1, n2){
    return n1 + n2;
};
utilsObj.subtract = function(n1, n2){
    return n1 - n2;
};
utilsObj.multiply = function(n1, n2){
    return n1 * n2;
};
utilsObj.divide = function(n1, n2){
    return n1/n2;
};
```

Testing the methods in the console was simple. Everything worked and he was now ready to begin.

```
//testing methods...
utilsObj.add(10, 2); //12
utilsObj.subtract(10, 2); //8
utilsObj.multiply(10, 2); //20
utilsObj.divide(10, 2); //5
```

He started creating his *object* using the *Object.create* method with the *prototype*(helper) object, *utilsObj* passed in as its parameter. He then added the *remainder* method to the resulting object - *myObj*. With the *remainder* method in, he had all the *arithmetic* operations he needed. 


```
//create a generic object
const myObj = Object.create(utilsObj);

//The remainder method
myObj.remainder = function(n1, n2){
    return n1 % n2;
};
```


He had read that by passing in the *prototype* object (*utilsObj*) during creation, would allow his object - *myObj*, to access methods of the prototype object. This, he needed to verify.

```
//testing...
//remainder method added in 
myObj.remainder(10, 2); //0 

//from the prototype utilsObj
myObj.add(10, 2)); //12
myObj.subtract(10, 2)); //8
myObj.multiply(10, 2)); //20
myObj.divide(10, 2)); //5
```

To his amazement he found that he not only could access the _remainder_ method that he had added, but also methods from the prototype object as if they belonged to his object - _myObj_! 

It was as if when the _add_ method was executed, JS looked up _myObj_. It did not find it and decided to check the prototype object - _utilsObj_. This felt more like inheritance via prototypes a.k.a. *prototypal inheritance*. 

He doubted this simplicity. To confirm, he decided to look under the hood. What he saw surprised him...

![myObj]( https://phptouch.files.wordpress.com/2019/12/myobj1.png "myObj - Under the hood")

There it was, the _utilsObj_ object, with the four arithmetic methods. His newly created object - _myObj_ with the new _remainder_ method that he had added. But, _myObj_ also contained a new property  - ___proto___ which looked identical to the _utilsObj_, the _prototype_ object! 

Bob knew he was onto something. It seemed that when JavaScript created _myObj_ it also added a new property, ___proto___ and pointed its value to the prototype object, _utilsObj_. So this is how _myObj_ could access methods in the prototype - _utilsObj_!

His excitement piqued with this new found knowledge he decided to do one more experiment - use _myObj_ as a prototype. So he created another object _appObj_ and passed in _myObj_ as the prototype object.

```
const appObj = Object.create(myObj);
```

First he confirmed whether he could access the _remainder_ method of _myObj_ which he had used as the prototype object. That worked fine.


```
appObj.remainder(10, 2); //0
```


Next he tested if methods of _utilsObj_ could be accessed. Remember _utilsObj_ was the prototype for _myObj_. They did.


```
//methods from utilsObj
appObj.add(10, 2); //12
appObj.subtract(10, 2); //8
appObj.multiply(10, 2); //20
appObj.divide(10, 2); //5
```


This started making sense to him. Could it be that one prototype object was linking to another, like a chain. To confirm he looked under the hood again.


![appObj](https://phptouch.files.wordpress.com/2019/12/appobj2.png "appObj - Under the hood")

Yes, his suspicions proved correct. It was indeed a chain. So this was the proverbial *prototype chain* that he had read about.

He could not believe it was all so logical and simple. He realized that "*prototype*" was just another name for a default or a helper object. Nothing more. Like Shakespeare's quote - *A rose by any other name would smell as sweet*.

Bob's quest complete, he meandered onto his next adventure…
