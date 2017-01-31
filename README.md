# JavaScript Best Practices

> A list of best approaches when it comes to coding in JavaScript.

-

Check out my [Blog](http://nitcrawler.blogspot.com) or say *hello* on [LinkedIn](https://bd.linkedin.com/in/arman-bhuiyan) / [Facebook](https://www.facebook.com/arman.it37).

## Table of Contents
-

* [Always Use === instead of using ==](#equality)

------------------------------------------------

## <a name="equality">Always Use === instead of using == .</a>

JavaScript offers two sets of equality operators: ==, != and ===, !==. Normally == is known as equality and === is known as identity(strict equality) operator.
To decide which one is best first we have to understand how differently both sets of operators work. In case of primitive type values equality operator(==) produces true if two operands have the same value. Even if the values are of different types JavaScript doesn't complain. Under the hood JavaScript does type coercion, meaning that the interpreter implicitly tries to convert the values to an expected type following some conversion protocols before comparing. Without further talking lets see some examples:

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

in case of reference type, JavaScript Objects are compared by reference, not by values. A JavaScript object is only equal to itself. It can't be equal to any other object even if that object has same number of properties, with the same names and values. Same rule works for Arrays. If two arrays have the same elements in the same order they are not equal to each other.

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
