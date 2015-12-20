---
layout: post
title: Test Driven Development (TDD)
---

Test-driven development or TDD is a powerful way of writing applications. It forces the programmer to write tests before any actual
code so they can have a clear idea of what exactly their program needs to do. It deepens their understanding of the problem they are
trying to solve. Another benefit of TDD is ensuring the previous code that's written does not get broken while being refactored, and
stating clearly the expectations that must be met at the same time. TDD is generally done through unit testing, which is testing a
program's smallest functionality at a time. This process is done in 3 simple steps:

### Write a failing test
```
describe('stack shared behavior', function(){

  it('reports a size of zero for a new stack', function() {
  expect(stack.size()).to.equal(0);

  it('reports a size of 2 after adding two items', function() {
    stack.push('a');
    stack.push('b');
    expect(stack.size()).to.equal(2);
  });

});
```
### Write the simplest code possible to pass the test
```
var Stack = function() {
  var obj = {};
  _.extend(obj, stackMethods);
  return obj;
}

var stackMethods = {
  constructor: Stack,
  size: function() {
    return 0;
  }
}
```
### Refactor the code
```
var Stack = function() {
  var obj = {};
  obj.count = 0;
  _.extend(obj, stackMethods);
  return obj;
}

var stackMethods = {
  constructor: Stack,
  size: function() {
    return this.count;
  },
  push: function(value) {
    this.count++;
  }
}
```
### Conclusion

As you can see this is a fairly simple process, but extremely valuable to you as a software engineer. Incorporating this methodology
in your future applications will return substantial results. It gives you a much more comprehensive understanding of your problem, safeguards your
previous code while you're refactoring, and documents expected behavior of the program. Go start using TDD!
