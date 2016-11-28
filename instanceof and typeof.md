- use `instanceof` for custome types
``` javascript
var ClassFirst = function () {};
var ClassSecond = function () {};
var instance = new ClassFirst();
typeof instance; // object
typeof instance == 'ClassFirst'; //false
instance instanceof Object; //true
instance instanceof ClassFirst; //true
instance instanceof ClassSecond; //false 
```

- use `typeof` for simple built in types
``` javascript
'example string' instanceof String; // false
typeof 'example string' == 'string'; //true

'example string' instanceof Object; //false
typeof 'example string' == 'object'; //false

true instanceof Boolean; // false
typeof true == 'boolean'; //true

99.99 instanceof Number; // false
typeof 99.99 == 'number'; //true

function() {} instanceof Function; //true
typeof function() {} == 'function'; //true
```

**A good reason to use typeof is if the variable may be undefined.**
```
alert(typeof undefinedVariable); // alerts the string "undefined"
alert(undefinedVariable instanceof Object); // throws an exception
```
**A good reason to use instanceof is if the variable may be null.**
```
var myNullVar = null;
alert(typeof myNullVar ); // alerts the string "object"
alert(myNullVar  instanceof Object); // alerts "false"
```

