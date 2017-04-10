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
`Sinon breaks up test doubles into three different categories: **spies**, **stubs**, and **mocks**, each with subtle differences.`


###### Sinon Spy
A spy in Sinon calls through to the method being spied on whereas you have to specify this behavior in Jasmine. For example:
```
spyOn(user, 'isValid').andCallThrough() // Jasmine
// is equivalent to
sinon.spy(user, 'isValid') // Sinon
```
In your test, the original user.isValid would be called.

The next type of test double is a stub, which acts as a controllable replacement. Stubs are similar to the default behavior of Jasmine spies where the original method is not called. For example:
###### Sinon Stub

```
sinon.stub(user, 'isValid').returns(true) // Sinon
// is equivalent to
spyOn(user, 'isValid').andReturns(true) // Jasmine
```
In your code, if user.isValid is called during the execution of your tests, the original user.isValid would not be called and a fake version of it (the test double) that returns true would be used.

#### when to use sinon with Jasmine
in many situations you won’t need to use Sinon if you are using Jasmine
One reason I do use Sinon with Jasmine is for its `fake server`


### Asynchronous Tests
Asynchronous testing in Jasmine 2.x and Mocha is the same.

### Sinon Fake Server
One feature that Sinon has that Jasmine does not is a fake server. This allows you to setup fake responses to AJAX requests made for certain URLs.
``` javascript
it('should return a collection object containing all users', function(done) {
  var server = sinon.fakeServer.create();
  server.respondWith("GET", "/users", [
    200,
    { "Content-Type": "application/json" },
    '[{ "id": 1, "name": "Gwen" },  { "id": 2, "name": "John" }]'
  ]);

  Users.all().done(function(collection) {
    expect(collection.toJSON()).to.eql([
      { id: 1, name: "Gwen" },
      { id: 2, name: "John" }
    ]);

    done();
  });

  server.respond();
  server.restore();
});
```
Sinon’s fake server is similar to the $httpBackend service provided in angular mocks.

### Conclusion
- The Jasmine framework has almost everything built into it including assertions/expectations and test double utilities (which come in the form of spies).

- Mocha is just a test runner and does not include assertion and test double utilities. 

- There are several choices for assertions when using Mocha, and Chai tends to be the most popular. 

- Test doubles in Mocha also requires another library, and Sinon.js is often the de-facto choice.
- Sinon can also be a great addition to your test harness for its fake server implementation.

---
Ref  
- Jasmine vs. Mocha, Chai, and Sinon  
http://thejsguy.com/2015/01/12/jasmine-vs-mocha-chai-and-sinon.html

- API documentation - Sinon.JS  
http://sinonjs.org/releases/v2.1.0/




