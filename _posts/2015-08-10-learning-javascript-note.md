---
layout: post
title: Javascript学习笔记(持续更新中)
---

![](/images/Bing_697.JPG)

##Conceptual Aside
### Syntax Parsers
A program that reads your code and determines what it does and if it's grammar is valid
(interpreter/compiler) do extra stuff

### Execution Contexts
A wrapper to help manage the code that is running

### Lexical Environments
where something sits physically in the code you write
(where you write something is important)

### Name/Value Pairs
A name which maps to a unique value

### Object
A collection of name value pairs

### The Global Environment And the Global Object
Global Object - like (window in browers)
'this'

Outer Environment
Global -> 'Not inside a Function'

### The execution context: creation and 'hoisting'
Hoisting: Setup Memory Space for Variables and Functions

All variable in javascript are initially set to undefined
And functions are sitting in memory in their entirety.

### Javascript and 'Undefined'
undefined: the variable hasn't been set, is javascript special value.

### The execution context: Code Execution

### Single Threaded:
one command at a time

### Synchronous Execution:
one at a time, in order

### Function Invocation And The Execution Stack
invocation: running a function, using parenthesis ()
execution context stack

### Variable Environments:
where the variables live:
every execution context has its own variable environment
how they relate to each other in memory

### The Scope Chain:
Scope: where a variable is available in your code.

### ES6
let -> block scope

### Asynchronous callbacks:

## Types:

### Primitive Types:
primitive type: a single value 就是一个独立的值
undefined 也是一个primitive type
js的number是浮点数，只有这一种数字类型
symbol是ES6中的新的类型

assignment is right to left:赋值操作的方向是从右往左

### coercion: converting a value from one type to another.
+ operator: coerce: number to string

Number(false) = 0
Number(undefined) = NaN
Number(null) = 0

### Equality ==:
"3" == 3   #true
false == 0 #true

### Strict Equality: ===  doesn't do coerce
绝大部分情况都是使用===进行比较，除非明确知道为什么要用==

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness

### Boolean
Boolean(undefined) #false
Boolean(null)      #false
Boolean("")        #false
Boolean(0) #false

### Default value:
name = name || "Default name"

## Objects and Functions
person['firstname']
person.firstname

var person = new Object();

### Object literal
var person = {};

### Fanking Namespaces:  Using Object
keep variables and functions with the same name separate

### JSON:
  JSON.stringfy(obj)
  JSON.parse("")

### Functions are objects
JS的functions也是对象,有自己的name属性等。

### First Class Functions:
Everything you can do with other types you can do with functions
Assign them to variables, pass them around, create them on the fly

CODE 也是function对象的一个属性 （Invocable）

### Expression:
A Unit of code that results in a value

### By value VS by reference:
primitive value: copy the value (by value)
all objects interact by reference

### Objects, Functions, 'this'
var self = this

### arguments and Spread:
arguments: the parameters you pass to a function
arguments.length
arguments[0]

function getPerson(){

  return {  #如果return后面没有{，JS的语法解释器会自动加上分号，导致函数就直接返回。
      firstname: 'tony'
    };
}

### Immediatiely Invoked function expressions

(function(name){
  retrun 'hello';
})();
() 里面就可以放入statement，所以可以放入function的statement

### Closure:
小心循环的陷阱
{% highlight javascript %}
function buildFunctions(){
  var arr = [];
  for(var i=0;i<3;i++){
    arr.push(
      (function(){
        var j = i;
        return function(){
          console.log(j);
        }
      }())
    )
  }
  return arr;
}

var fs = buildFunctions();
fs[0]();
fs[1]();
fs[2]();

function buildFunctions(){
  var arr = [];
  for(var i=0;i<3;i++){
    arr.push(
      (function(j){
        return function(){
          console.log(j);
        }
      }(i))
    )
  }
  return arr;
}

var fs = buildFunctions();
fs[0]();
fs[1]();
fs[2]();
{% endhighlight %}

### Function Factories

### Closures And Callbacks
callback function: A function you give to another function to be run when the other function is finished.

### Call(), Apply(), Bind()
bind(obj): 返回一个copy的function，然后把function中的this赋值为obj

call(obj, '',...):会设置this为obj然后执行这个function

apply(obj, []):跟call类似，就是参数需要是array

function borrowing

function currying
  使用bind可以预设参数值

  Creating a copy of a function but with some preset parameters
  very useful in mathematical situations

### Functional Programming
  underscore.js -> source code # TODO
  lodash.com

## Object-Orientied Javascript and prototypal inheritance

每个obj都有一个proto属性，指向其prototype

prototype chain:

// don't do this EVER!
john.__proto__

Everything is an object (or a primitive)

### base object : Object

function的prototype -> function Empty(){}  #apply(), bind(), call()

### Reflection and Extend
  An object can look at itself, listing and changing its properties and methods.
{% highlight javascript %}
  for(var prop in john){
    if(john.hasOwnProperty(prop)){
      console.log(prop + ": " + john[prop]);
    }

  }
  underscore.js
    _.extend(john, jane, jim)
    to combine and compose other objects
{% endhighlight %}

## function Person(){} # function constructor
A normal function that is used to construct objects.

The 'this' variable points a new empty object, and that object is returned form the function automatically.

Function的prototype属性只有是使用new的情况下次才会用到。


### Built-in function constructors
{% highlight javascript %}
var a = new Number(3);
a.toFixed(2);

var b = 3

a==b #true
a===b #false

moment.js -> Date lib

for array iterating overall properties is not save
Array.prototype.mycustomFeature = 'cool';
var arr = ['a', 'b', 'c'];
for(var prop in arr){
  console.log(prop + ':' + arr[prop]);
}
{% endhighlight %}

### Object.create and Pure Prototypal Inheritance
{% highlight javascript %}
var person = {
  firstname: 'Default',
  lastname: 'Default',
  greet: function(){
    return 'Hi ' + this.firstname;
  }
}

var john = Object.create(person);
john.firstname = 'John';
john.lastname = 'Doe';
console.log(john);

{% endhighlight %}

### Polyfill
Code that adds a feature which the engine may lack
{% highlight javascript %}
if (!Object.create) {
  Object.create = function (o) {
    if(argument.length > 1) {
      throw new Error("Object.create implementation only accepts the frist parameter.");
    }
    function F() {}
    F.prototype = o;
    return new F();
  };
}
{% endhighlight %}

### ES6 and CLASSES
extends

classes in javascript a phrase that it's just syntactic sugar

## ODDS and ENDS

### typeof, instanceof
typeof 3 #number
typeof 'hello' #string
typeof {} #object
typeof [] #object
Object.prototype.toString.call(d) #[object Array]

instanceof #检查prototype chain

typeof (function{})  #function

### Strict mode
"use strict";

function myfunc(){
  "use strict";

  ....
}
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode

### Open Source Education

a dollar sign
isNumberic

sizzle

it's ok to return something from the function constructor (default is this)
可以在返回之前对this做一些操作，然后用return明确得返回

### Transpile:
  Convert the syntax of one programming language, to another.

TypeScript

Traceur: ES6 -> ES5

To read a list of features existing or coming in ES6, head here: https://github.com/lukehoban/es6features















