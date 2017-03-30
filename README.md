# JavaScript-interview-preparation
My preparation for getting a full time job as a front end developer

* The null gotcha!

  null datatype qualifies as an Object.
  ```
  const bar = null;
  console.log(typeof bar === "object"); //returns true!
  ```

  If you want to reliably check if bar is an object, check for null condition first.
  ```
  console.log((bar !== null)&&(typeof bar === "object"));
  ```

  1. If bar is a function, the above code will return false.
  If you want to return true for bar being a function, simply add the following with an and condition
  ```
  (typeof bar === "function")
  ```
  
  2. Arrays are Objects too, so the expression will return true for arrays. 
  To return false for arrays, use the toString.class() method:
  ```
  (toString.call(bar) !== "[object Array]"))
  ```
 
* Since toString is defined in Object.prototype, whoever inherits Object, will by default get the toString method.

  But, Array objects, override the default toString method to print the array elements as comma separated string.
  [source](http://stackoverflow.com/questions/30010996/difference-between-object-prototype-tostring-callarrayobj-and-arrayobj-tostrin)
  
  toString() can be used with every object and allows you to get its class using the `call` function.
  ```
  var toString = Object.prototype.toString;

  toString.call(new Date);    // [object Date]
  toString.call(new String);  // [object String]
  toString.call(Math);        // [object Math]

  // Since JavaScript 1.8.5
  toString.call(undefined);   // [object Undefined]
  toString.call(null);        // [object Null]
  ```
  [source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)

* `var a = b = 3;` is equivalent to 
  ```
  b = 3;
  var a = b;
  ```
  
  If the statement was enclosed within a function
  
  ```
  function funk(){var a = b = 3;}
  console.log(b);// We get 3
  console.log(a);//we get undefined
  ``` 
  
  b would be accessible and defined outside the function, but a would not be defined in the global scope because it has a var prefix in the declaration
  
  If the statement had const instead of var, neither of the variables would print outside the block's scope

* Coercion

  Type coercion means that when the operands of an operator are different types, one of them will be converted to an "equivalent" value of the other operand's type. For instance, if you do:

  ```
  boolean == integer
  ```
  the boolean operand will be converted to an integer: false becomes 0, true becomes 1. Then the two values are compared.

  However, if you use the non-converting comparison operator ===, no such conversion occurs. When the operands are of different types, this operator returns false, and only compares the values when they're of the same type.
  [source](http://stackoverflow.com/questions/19915688/what-exactly-is-type-coercion-in-javascript)
  
  Another example:
  
  ```
  [] + 5
  ```
  In JavaScript, + can mean add two numbers or concatenate two strings. In this case, we have neither two numbers nor two strings. We only have one number and an object. 
  Javascript knows how to concatenate strings, so it converts both [] and 5 into strings and the result is string value “5”.
  
* IIFEs are executed before other functions and have their own lexical scope which helps prevent the variables from leaking.
* IIFEs may get expressed in an expression before variables trickle down to their scope.

  ```
  var myObject = {
      animal: "bear",
      func: function() {
          (function() {
              console.log(this.animal, this);
          }());
      }
  };
  myObject.func();
  ```
  `this` inside of a `function` will always equal the global object (`window`) unless you change the context of `this` via `.bind()`, `.call()` or `.apply()`.
  
  This function returns `undefined` for 'this.animal' and returns `Window` object for 'this'.
  Because animal is not present in the global scope and is limited to the object, it gets a value of undefined.
