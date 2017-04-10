## Understanding JavaScript testing frameworks


两种主要的javascript framework
- Jasmine 2
- Mocha
Chai and Sinon, that are often used in conjunction with Jasmine and Mocha.


The APIs of Jasmine and Mocha are very similar. They both allow you to write your tests in the behaviour driven development (BDD) style. You might ask, “What is BDD?”. In short, BDD is simply a style of writing tests that focusses on the language used.

Jasmine
``` javascript
expect(calculator.add(1, 4)).toEqual(5);
```

Chai
``` javascript
expect(calculator.add(1, 4)).to.equal(5);
```






---
Ref
Jasmine vs. Mocha, Chai, and Sinon
- http://thejsguy.com/2015/01/12/jasmine-vs-mocha-chai-and-sinon.html

http://sinonjs.org/


