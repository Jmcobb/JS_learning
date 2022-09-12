## NOTES FOR JS FUNCTIONS

<hr>

## What is a function?

These are one of the key building blocks of JavaScript. Specifically, a block of code that takes in an input and returns and output.

` (Input) -> stuff happens -> (Output)`

## How do you define a function?

Using the function keyword you need to add the following to define a function:

- Name of the function.
- Some input in a parentheses (if multiple separate by using a `,`)... sometimes called a parameter
- And `{}` or body of the function where the stuff that happens goes.

It'll look something like this:

```
function NameHere(input_1, input_2) {
    stuff_happening_to_make_output;
    return output;
}
```

##### Parameters - Non_Array vs. Array and Objects:

If the input, or parameter, of the function is passed in by type... so if your function changes the value of the parameter it will not be reflected globally, or in the original code that the value was assigned. Only inside of your function.

However, if the param of the function is a array then the change will be reflected outside the scope of the function. This also extends to object values as well.

### Function expressions

Another way to define a function is via an expression. Some cool things this does is allow the function to be anonymous and not require a name like we previously talked about.

```
const NamelessFunc = function (input) {
    output = input - 2
    return output;
};
```

notice how the function its self does not have the name but the expression is named: `NamelessFunc`

> MDN docs call for using a name in cases of debugging to see the stack trace as otherwise it would not be seen.

A strength of the expression functions are the ability to pass a function to another function.

```
const add = function(x + y){
    return x + y
}

function sumTimesTwo(add){
    return sum * 2
}
```

## How do you call a function?

Just because a function is defined does not mean it will run. You actually need to call the function after defining to preform the predetermined steps in the function.

```
//Defined function
Function MultiplyByItself(x){
    return x * x
};

//Actually call by:
MultiplyByItself(6)
```

The statement above, with the params, actually runs the previously defined function. Specifically, we are doing: `Name(prams4func)`.

Note: when declaring a function that function needs to be in scope, or in predefined before calling and executing. (P.S. Specially hoisting functions will not work on expressions only declared functions.)

Also, you can pass whatever you want into a function! Objects, Arrays, or other functions!

Another fun quirk is functions are able to call themselves, you can see this happen in conditionals.

## Does the function have it's own scope?

Yes! Specifically, when dealing with functions the `{function body}` create the functions own scope.

That means that when a function is defined in the global scope it has access to the rest of the global scope, but, any variables defined inside the function or it's scope will only be accessible in the function

```
Variable = 'Paul'
Verb = 'a lot'

function setName(name, verb){
    Function_Scope_variable = 'Sally'
    Function_Scope_verb = 'none'

    return name + ' loves me ${verb}'
};

setName() -> 'Paul loves me a lot'
setName(Function_Scope_variable, Function_Scope_verb) -> error
```

the following `setName(Function_Scope_variable, Function_Scope_verb)` would only be viable if called inside of the function itself.

## Scoping + the function stack

### Recursion

A recursive function is a function that calls itself. Think of a loop, but like a function. Both need run restrictions to prevent infintie runs, and code to run multiple times.

Any function can call itself in 3 ways:

1. name of function()
2. name of expression()
3. arguments.callee

_Why would I do this instead of a loop?_
Well... good question, a good example would be to use Javascript for dom manipulation to do a certain number of things.
Like this following example:

```
function walkTree(node) {
  if (node === null) {
    return;
  }
  // do something with node
  for (let i = 0; i < node.childNodes.length; i++) {
    walkTree(node.childNodes[i]);
  }
}
```

(\*Directly pulled from: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#scope_and_the_function_stack)

Recursion is a useful tool and like loops preforms a stack like way of knocking things out.

### Recursion and closures

Sometimes it can be very valuable to nest a function inside of another, this forms a _closure_

Additional closure video: https://www.youtube.com/watch?v=3a0I8ICR1Vg

A simplified way to think about it is like this: If there is an inside function, it can access all of the variables available to he scope of the parent/outside function.

```
x = 6
y = 7

Function outerFunction(x) {
    function innerFunction(y){
        const non_accessible_to_parent = "Secret"
        return x * y
    }
    return InnerFunction
}

```

note: The parent function cannot access any variables inside the innerFunction. See `non_accessible_to_parent` in the example above.

### Variable memory and recursive closures

A closure needs to be ready to take on multiple calls/arguments so every time a new closure is made it acts as another stored variable in memory, much like objects. We actually exactly like objects but you can't call and log them so less right in front of your face.

### Scope chaining

This is basically when you have some nested functions that have multiple scopes.

```
function A(x) {
  function B(y) {
    function C(z) {
      console.log(x + y + z);
    }
    C(3);
  }
  B(2);
}
A(1); // logs 6 (1 + 2 + 3)

```

> Couldn't think of a good example: so I pulled the above from: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#scope_and_the_function_stack

#### Naming conflict in recursion

Furthering on scope chain what does that look like? What happens if we have variables named the same as a inner function?

Those kind of examples actually refer to the "scope chain", lets look at an example where you want to find out what is the age of your baby in months from years:

```
function bbyName(name){
    const x = 12
    function plusAge(x) {
        age = x * 24
        name + ' ' + age + ' months old'
    }
    return plusAge
}

bbyName('Chris S.')(2) // would log "Chris S. 48 months old"
```

The output above is incorrect because the innermost scope is king! So the deeper the scope the more prescience it takes.

## Closure recap

- Very powerful tool!
- Ability to nest functions.

```
function Boop() {
    function Beep() {

    }
}
```

- Inside function can access all variables of outside function and those scoped outside the function.
- Outside function does not have access to any variables of inside function.
- Inside function lives longer than outside function
  > This is because the inside function has access to the outer scoped variables, if the inner function manages to survive longer than the outside function then they are basically stored like an object on the machine. This is also a great way to store new variables as they will live longer and be stored past the initial life of the outside function.

## The Arguments Object

The active maintenance of function is equal to an array-like object! What does that even mean? That's like 2 different data types...

`arguments[i]`

Using this you can actually call a function with more arguments than you would normally be allowed too, which is pretty sweet when you don't know the number of functions it has.

Possibly use `.length` (like an array) to see the number of arguments then access it like the object it really is. This sounds a lot like it would be good for data manipulation:

```
function separateData(list) {
    let result
      for (let i = 1; i < arguments.length; i++) {
    result += arguments[i] + separator;
  }
  return result;
}

separateData(',', 'Billy', 'Dale', 'Joe')
// prints ("Billy, Dale, Joe")
```

Note: Cannot use all array manipulation methods as it is only array-like, not actual array.

## Function Params

JS has 2 special kinds of parameters: _default_ and _rest_.

### Default parameters

Naturally, undefined params in JS are set to `undefined` and when working with data if you don't get a value it's proably a good idea for some error handling but, anyways, it's always good to know what is going on with your app!

```
func multiply(x, y = 1) {
// default param of y = 1
    return x * y
}

multiply(6) // returns '6'
```

### Rest Params

This allows us an undefined number of arguments as an array!

```
function multiply(multiplier, ...theArgs) {
  return theArgs.map((x) => multiplier * x);
}

const arr = multiply(2, 1, 2, 3);
console.log(arr); // [2, 4, 6]
```

Basically, the function above multiplies the args passed into an array and multiplies them out.

## Arrow functions

The "Fat" arrow, or arrow function is a newer implementation to javascript. This has shorter syntax, which is great for cleaner code, but looses the `this`, `arguments`, `super`, and `new.target` functions JS. Also, it is ALWAYS anonymous.

```
() => {}
```

There are 2 major factors for it's implementation: readability, and the non-binding of `this`.

### Free the `this`

until arrow functions every function has it's own predefined `this`.

> this => basically a new object

That became not the best when working in tandem with OOP or object oriented programming styles. So, now a arrow function does not contain it's own this to help further better building syntax around the OOP model.

```
function Dog() {
  this.age = 0;

  setInterval(() => {
    this.age++; // `this` properly refers to the person object
  }, 7);
}

const p = new Dog();
```

## Predefined functions:

Javascript has a good number of built in predefined functions.

`TODO: Make some flask cards`

<hr>

```
eval()
```

The eval() method evaluates JavaScript code represented as a string.

<hr>

```
isFinite()
```

The global isFinite() function determines whether the passed value is a finite number. If needed, the parameter is first converted to a number.

<hr>

```
isNaN()
```

The isNaN() function determines whether a value is NaN or not. Note: coercion inside the isNaN function has interesting rules; you may alternatively want to use Number.isNaN() to determine if the value is Not-A-Number.

<hr>

```
parseFloat()
```

The parseFloat() function parses a string argument and returns a floating point number.

<hr>

```
parseInt()
```

The parseInt() function parses a string argument and returns an integer of the specified radix (the base in mathematical numeral systems).

<hr>

```
decodeURI()
```

The decodeURI() function decodes a Uniform Resource Identifier (URI) previously created by encodeURI or by a similar routine.

<hr>

```
decodeURIComponent()
```

The decodeURIComponent() method decodes a Uniform Resource Identifier (URI) component previously created by encodeURIComponent or by a similar routine.

<hr>

```
encodeURI()
```

The encodeURI() method encodes a Uniform Resource Identifier (URI) by replacing each instance of certain characters by one, two, three, or four escape sequences representing the UTF-8 encoding of the character (will only be four escape sequences for characters composed of two "surrogate" characters).

<hr>

```
encodeURIComponent()
```

The encodeURIComponent() method encodes a Uniform Resource Identifier (URI) component by replacing each instance of certain characters by one, two, three, or four escape sequences representing the UTF-8 encoding of the character (will only be four escape sequences for characters composed of two "surrogate" characters).

<hr>

```
escape()
```

The deprecated escape() method computes a new string in which certain characters have been replaced by a hexadecimal escape sequence. Use encodeURI or encodeURIComponent instead.

<hr>

```
unescape()
```

The deprecated unescape() method computes a new string in which hexadecimal escape sequences are replaced with the character that it represents. The escape sequences might be introduced by a function like escape. Because unescape() is deprecated, use decodeURI() or decodeURIComponent instead.

<hr>
