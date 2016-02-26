# Javascript Basics

The easiest way to get practice with Javascript is to use node.  My directions for installing nvm are [in the gng-mobile readme](https://github.com/Wirestorm/costco-gng-mobile). Once you have a version of node installed, you can run some Javascript code in a couple of different ways.  You can simply type `node` from the command line and you will open a Javascript shell, which you can exit by either entering `.exit` or entering `ctrl-C` twice. You can alternately write the code you want to test in a file such as `exampleFile.js` and run it from the command line by typing `node exampleFile.js`.


#### Tip

- If you want to print any value to the console, simply run `console.log(<value>)`.  This is a very useful command for troubleshooting and for understanding what is going on with Javascript code.

#### What are es6 and es7?

- The [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) (es) specification is a name for the particular version of JavaScript that is being used.  Browsers currently support es5, but we use es6 (and some things that are technically part of es7) here at Wirestorm.  We use [babel](https://babeljs.io/) as a transpiler to convert es6 code (as well as some code from es7) into a syntax that browsers will recognize.

## JavaScript constants

The constants that are built into JavaScript as global values are `undefined`, `NaN`, and `infinity`.  The values `null`, `true`, and `false` are all recognized by JavaScript, even if they are not officially globally defined constants.

## Declaring and using variables

All variables in Javascript are declared using the `var <name> = <value>` syntax.  If you want to see an example, make a file containing
```
var variable1 = 'example string';
var variable2 = false;
var variable3;
console.log(variable1, variable2, variable3);
// 'example string' false undefined
```
and run it from the console.

#### es6/es7 note

- es6 actually uses two commands in place of `var`.  `let` is used for variables that are allowed to change their values, and `const` is used for variables that will never change value.

#### When do I use a semicolon?

- The semicolon is supposedly required at the end of every line of code.  This means that it *should* be used after every statement that begins with `var` and after every function call.  This is the standard adopted by Wirestorm, but your JavaScript code will run even if you don't include a single semicolon and some style guides will completely omit semicolons.

The types of data in Javascript are 'undefined', 'number', 'string', 'boolean', 'object', and 'function'. You can determine the type of a variable with the `typeof(<value>)` syntax.

Any number, string, or boolean is immutable, meaning that its value is fixed once it is declared.  Objects and functions are mutable on the other hand, meaning that these variables reference a location in memory, but the data there can change.

#### Useful references

- The different types of data in JavaScript roughly correspond to the various (Global objects)[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects] that are built-in.  Each built in object has a prototype

### Objects and Arrays

An Object is declared as a list of `<key>: <value>` pairs surrounded by `{` and `}`.  Some examples of object declarations are
```
var object1 = {};
var object2 = {
  exampleKey: 'example value',
  'exampleKey2': 'example value 2',
  "example_3": false
};
var object3 = { property1: 1, property2: 2 }
```
The values of an object can be referenced by either `<object>.<propertyName>` or `<object>[<string>]`.  For example, you can view the values of the objects above with the commands
```
console.log(
  object1.anything, // undefined
  object2['exampleKey'], // 'example value'
  object2.exampleKey2 // 'example value 2'
);
```

#### Useful references

- The different types of data in JavaScript roughly correspond to the various [Global objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects) that are built-in.  **Each built in object has a prototype property that is inherited by the values with that type.** These values may be referenced just like object properties.  For example, String.prototype has the value [String.prototype.toUpperCase](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) so any string will have this property (e.g. `'a string'.toUpperCase() // returns: 'A STRING'`).  I regularly look up the prototypes for [Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype), [Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/prototype), and [String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/prototype).

An array is declared as a list of values surrounded by `[` and `]`, and its values are referenced by `<array>[<index>]`.  For example
```
var array1 = [];
var array2 = ['index 0', 'index 1', 'index 2'];
var array3 = ['dog', 'cat', 'llama'];
console.log(
  array1[1], // undefined
  array2[0], // 'index 0'
  array2[2], // 'index 2'
  array3[1] // 'cat'
);
```

### Functions

Functions may be declared in two ways, and these types of declarations behave subtly differently.  A function may be declared just like any other variable
```
var function1 = function(argument1, argument2) {
  //... here is what the function does.
};
```
or it may be declared with the syntax
```
function function2 (argument1, argument2) {
  //... here is what the function does.
}
```
When the function is called it will execute any code between the curly brackets until it encounters a line beginning with `return`.

The difference between these notations is that the special `function <name> (<arguments>){...}` syntax is run before any of the other code in the file.  This means that
```
var result = exampleFunction(5);
function exampleFunction(number) {
  return number * 3;
}
console.log(result);
```
will run and you will see `15` displayed in the console, but
```
var result = exampleFunction(5);
var exampleFunction = function(number) {
  return number * 3;
}
console.log(result);
```
will throw an error because `exampleFunction` is undefined on line 1.

A function may also be set as a property of an object.  Such functions are referred to as methods.  For example, the object
```
var object = {
  method: function() {console.log('the method was called');}
};
```
has a method.  If we then run the method, we will see
```
object.method() // 'the method was called'
```

### Context and `this`

Every function has a context, which is represented by an object called `this`.  If a function is set as the method of an object, then the object becomes the context of the function.  For example
```
var exampleObject = {
  name: 'example object',
  showName: function() {console.log(this.name);}
};
exampleObject.showname(); // 'example object'
```
You can even set a method equal to a function that is declared outside of the object and it will keep the object as its scope.
```
this.name = 'global context';

var object = {
  name: 'object context',
  showName: showName
};

function showName() {
  console.log(this.name);
}

showName(); // 'global context'
object.showName(); // 'object context'
```
But what if you want the function to keep the global context?  You can use the `Function.prototype.bind` method to change the function's context to the parent context.
```
this.name = 'global context';

var object = {
  name: 'object context',
  showName: showName.bind(this)
};

function showName() {
  console.log(this.name);
}

showName(); // 'global context'
object.showName(); // 'global context'
```
