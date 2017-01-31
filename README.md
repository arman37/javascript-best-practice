# JavaScript Best Practices

> A list of best approaches when it comes to coding in JavaScript.

-

Check out my [Blog](http://nitcrawler.blogspot.com) or say 'hello' on [LinkedIn](https://bd.linkedin.com/in/arman-bhuiyan) / [Facebook](https://www.facebook.com/arman.it37).

## Table of Contents
-

* [Always Use === instead of using ==](#equality)
* [Strict Mode - Wrap your js file with IIFE](#strict__mode)

------------------------------------------------

## <a name="equality">Always Use === instead of using == .</a>

JavaScript offers two sets of equality operators: ==, != and ===, !==. Normally == is known as equality and === is known as identity(strict equality) operator.
To decide which one is best first we have to understand how differently both sets of operators work. In case of primitive type values, equality operator(==) produces true if two operands have the same value. Even if the values are of different types JavaScript doesn't complain. Under the hood JavaScript does type coercion, meaning that the interpreter implicitly tries to convert the values to an expected type following some conversion protocols before comparing. Without further talking lets see some examples:

```javascript
false == 0; //true, because 'false' is converted to 0 and then compared.
"5" == 5;  //true, because "5" is converted to 5 and then compared.
```

lets dive into a little more confusing examples:

```javascript
'' == '0'           // false
but,
0 == ''             // true

false == 'false'    // false
but,
false == '0'        // true

false == undefined  // false
false == null       // false
but,
null == undefined   // true
```
On the other hand, identity operator(===) produces true only if both operands have same value and are of same type. In that case JavaScript doesn't type coercion. so if two values are not the same type identity operator will simply return false. Lets see it in action:

```javascript
true === 1; //false
"2" === 2;  //false
```

In case of reference type, JavaScript Objects are compared by reference, not by values. A JavaScript object is only equal to itself. It can't be equal to any other object even if that object has same number of properties, with the same names and values. Same rule works for Arrays. If two arrays have the same elements in the same order they are not equal to each other.

```javascript
var
  a = { x: 1, y: 2 }
  ,b = { x: 1, y: 2 }
  ,c = a;

  a === b; // false (even though a and b are the same type, have same number of properties, with same names and values.)
  but,
  a === c; // true
  or
  a == c; // true
```
No matter what operator you use, if both operands don't refer to the same object, they are not equal.

The rule is:

For primitive types:
  x === y returns true if x and y have the same value and are of the same type.

For reference/object types:
  x === y returns true only if x and y refer to the exact same object.

For strings:
  x === y returns true if a and b are both strings and contain the exact same characters
but,
var x = 'abc';
var y = new String('abc');
so x === y returns false because both operands are not the same type. x is type of string while y is type of object.

One thing to notice here is that, identity operator(===) is faster than equality operator(==) in some cases. When both operands have values of same type, both operators take same time. But if operands have values of different types, identity operator doesn't do type conversion but equality operator does.

## <a name="strict__mode">Strict Mode - Wrap your js file with IIFE.</a>

We know that according to ECMAScript version 5(JavaScript 1.8.5) specification we can use a new directive known as "use strict" to make it easier to write more secure JavaScript code, to place our code into a more constrained form of execution. With strict mode enabled we get to avoid some insecure, ill-advised, pretty bad syntax errors that early version of browsers used to forgive, like trying to assign a value to an undeclared variable, accidentally creating a new global variable by mistyping a variable name, assigning a value to a readonly object property, using a non-existing object property, duplicating variable names, trying to delete a variable or a function or an undeletable object property, using 'eval' or 'arguments' as variable name, using deprecated language features etc.

Since strict mode is standard and a best practice and maybe in the future it'll be our only choice, we have to learn how to use it properly. Strict mode can be used in two ways. One way is using strict mode at the file level and another way is using it at the function level. In both ways the string literal 'use strict' must be placed at the beginning of file/script tag/function body. Since strict mode is only recognized at the top of file/script tag/function body, it causes problem for script concatenation. Placing strict mode in a global context means all your code will run in strict mode. Imagine you have two files named 'A' where strict mode is enabled and 'B' where strict mode is not enabled.

```javascript
// A.js
'use strict';
let foo = () => {

};

// B.js
let bar = () => {

};
```

During script concatenation if you place file A before file B producing file named 'AB' then the code in file B will also run in strict mode.

```javascript
// AB.js
'use strict';
let foo = () => {

};

let bar = () => {

};
```
If you place file B before file A producing file named 'BA' then none of your code will run in strict mode.

```javascript
// BA.js
let bar = () => {

};

'use strict';
let foo = () => {

};
```

If your application is small then you can choose 'strict mode only' or 'non-strict mode only' policy. But if your application starts to grow and you are using lots of third party libraries then you gotta come up with a better solution. You can concatenate all strict files and non-strict files in two separate files. But you'll loose control over the file structure of your application, which is bad. The best possible way out there so far is wrapping each file with immediately invoked function expression(IIFE). This way every file will be independently interpreted in different modes.

```javascript
// BA.js
(function() {
  'use strict';
  let foo = () => {

  };
}());

// B.js
(function() {
  let bar = () => {

  };
}());
```
One thing to notice though is that none of the contents of the file can assume that they are interpreted at global scope.
