## Understanding JavaScript testing frameworks


两种主要的javascript framework
- Jasmine 2
- Mocha
Chai and Sinon, that are often used in conjunction with Jasmine and Mocha.

### API
The APIs of Jasmine and Mocha are very similar. They both allow you to write your tests in the behaviour driven development (BDD) style. You might ask, “What is BDD?”. In short, BDD is simply a style of writing tests that focusses on the language used.

Jasmine
``` javascript
expect(calculator.add(1, 4)).toEqual(5);
```

Chai
``` javascript
expect(calculator.add(1, 4)).to.equal(5);
```

#### Test Doubles
Test doubles are often compared to stunt doubles, as they replace one object with another for testing purposes
类似于演员的替身。

##### Spies

 In Jasmine, test doubles come in the form of spies. A spy is a function that replaces a particular function where you want to control its behavior in a test and record how that function is used during the execution of that test.

Some of the things you can do with spies include:
- See how many times a spy was called
- Specify a return value to force your code to go down a certain path
- Tell a spy to throw an error
- See what arguments a spy was called with
- Tell a spy to call the original function (the function it is spying on). By default, a spy will not call the original function.

In Jasmine, you can spy on existing methods like this:
``` javascript
var userSaveSpy = spyOn(User.prototype, 'save');
```
Mocha does not come with a test double library. Instead, you will need to load in Sinon into your test harness. 

`Sinon is a very powerful test double library and is the equivalent of Jasmine spies with a little more.`



---
Ref  
Jasmine vs. Mocha, Chai, and Sinon
- http://thejsguy.com/2015/01/12/jasmine-vs-mocha-chai-and-sinon.html

http://sinonjs.org/


