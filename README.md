# Road2Js
A compilation of useful JS infos I have acquired throughout my journey of diving into JS as a Java developer.


## Hoisting
JavaScript puts function declarations into memory before it executes any block of code. This allows us to use functions even before declaring them. 

```javascript
// Prints "hello" even if printHello was just declared after this statement
printHello();

function printHello() {
    console.log("hello");
}
```

Hoisting will make it look like this:
```javascript
function printHello() {
    console.log("hello");
}

printHello();
```

It is also good to note that only **function declarations** are hoisted and **not function expressions**.
```javascript
printHello();

const printHello = function() {
    console.log("Hello");
};
```

Variable declarations are also hoisted. Take note that initialization is different from variable declarations.

```javascript
var test = "hello"; // variable declaration
var testTwo;
testTwo = 12; // initialization
```
A good thing to note is that **only the variable declaration are actually hoisted, not the actual value given to the variable itself**.

There's a difference between `var`, `let` and `const`. `const` will throw an error if no initial value was assigned on declaration. On the other hand, `var` and `let` allows initialization without a value. However, they throw out different errors. `let` throws a `ReferenceError` if it was referenced with no value at all while `var` gives you a value of `undefined`.

```javascript
console.log(test); // prints undefined
var test = "hello";
```

Turns out it would look like this:
```javascript
var test;
console.log(test);
test = "hello";
```
For a deeper explanation, check out `Sukhjinder Arora`'s [medium article](https://blog.bitsrc.io/hoisting-in-modern-javascript-let-const-and-var-b290405adfda).

## Modern JavaScript (ES6, ES7, ESNext...)
First, let us define what is ECMAScript (abbreviated as ES). In a nutshell:
>ECMAScript is a **standard**. JavaScript is a well-known implementationsof the ECMAScript standard. 
by DavidRR from https://stackoverflow.com/questions/4269150/what-is-ecmascript

*Note that the following sections would only be highlighting some of the notable features released on that period.*

#### History
##### ECMAScript 6 (ES6) or ECMAScript 2015
* Syntactic sugar on classes
* Modules
* Iterators and `for of`
* Arrow functions `() => {}`
* `let` and `const` keyword
* Promises
* Template literals `Hello there ${variableName}`
* Default value on parameters
* Rest and Spread operator `...`
* Additional data structures (Map, Set, WeakMap, WeakSet)

##### ECMAScript 2016 (sometimes referred to as ES7)
* `Array.prototype.includes`
* Exponentiation Operator `**`
* Block scoping of variables and functions

##### ECMAScript 2017 (sometimes referred to as ES8)
* `async`/`await` to avoid callback hell
* `Object.values` and `Object.entries`

##### ECMAScript 2018 (sometimes referred to as ES9)
* Regex features
* Rest/Spread properties

##### ECMAScript 2019 (sometimes reffered to as ES10)
* `Array.prototype.flat`
* `Array.prototype.flatMap`

##### ESNext
A dynamic name for the generation or edition of ECMAScript. These are mostly proposals from developers and contributors.

#### Modern Coding Approach
* Make use of spread operators on Objects and Arrays
```javascript
const obj = { name: "Joshua", email: "test", age: 12 };

// Suppose we wanted to update only one field
const obj2 = { name: obj.name, email: obj.email, age: obj.age + 1 };

// A more robust approach:
const updatedObj = { ...obj, age: obj.age + 1 };
// This allows you to save yourself from typing added properties in the future
```

```javascript
const originalArray = [ 1, 2, 3, 4, 5 ];
const secondOriginalArray = [ 'a', 'b', 'c' ];

// Suppose I wanted to include the elements of these arrays within a new array obj
const newArray = [ 'letters', ...secondOriginalArray, 'numbers', ...originalArray ];
```

```javascript
const myName = (firstName, middleName, lastName) => {
    console.log(firstName + middleName + lastName);
}

const fullName = [ "John Joshua", "T" "Jimenez" ];

// suppose we wanted to call myName and pass in the elements of fullName
myName(fullName[0], fullName[1]), fullName[2];

// lets save you some typing and do it like
myName(...fullName);
```

* Use the improved versions of `for`: `for...of` on list of values and `for...in` and on list of keys on an object

```javascript
const numbers = [ 1, 2, 3, 4, 5 ];
for(const number of numbers) {
    console.log(number); // outputs array values, 12345
}

for(const number in numbers) {
    console.log(number); // outputs array indices, 0, 1, 2, 3, 4
}

// or if you wanted the functional style...
numbers.forEach(number => console.log(number));
```

but what if you needed the good old for loop's index? Worry not.

```javascript
const numbers = [ 1, 2, 3, 4, 5 ];
for(const [ index, emtry ] of numbers.entries()) {
    console.log(`Index ${index} has a value of ${entry}`);
}
```

* Use **arrow functions** carefully. Regular function declarations are lexically scoped (except for `this` and `arguments`). Arrow functions on the other hand are purely lexically scoped which includes `this` and `arguments`. A bad situation on using arrow functions is when inside a `class`, which usually requires its methods to have dynamic binding.

* Ever done a long string concatenation? Something like this? `"test" + varOne + " " + "hello"` You just got saved, as template string literals are now a thing in JavaScript.

```javascript
const ONE = 1;
const TWO = 2;

console.log(`This is number ${ONE} and this is number ${TWO}`)
```

* Utilize object literals when assigning variables to object properties having the same name.

```javascript
const name = "Josh";
const age = "21";
const email = "test";

// this is cumbersome
const josh = {
    name: name,
    age: age,
    email: email
};

// and to save some typing:
const shorterJosh = {
    name, age, email
};
```

* Object and array destructuring is your friend when you would like to get only specific elements from these data structures.

```javascript
const strings = [ "hello", "there", "test" ];

// Suppose we wanted to get elements 0 and 1
const str1 = strings[0];
const str2 = strings[1];

// A one liner shorthand would be
const [ str3, str4 ] = strings;
console.log(str3); // outputs "hello"
console.log(str4); // outputs "there"
```

```javascript
const person = {
    name: "Person",
    age: "69",
    address: "PH"
};

// Suppose we wanted to assign each property of person to its own variable
const name = person.name;
const age = person.age;
const address = person.address;

// One liner shorthand
const { name, age, address } = person;

// and if you wanted to give the variable a different name
const { name: personName, age: personAge, address: personAddress } = person;
```

and if you wanted to be more creative, combine it with the spread operator!

```javascript
const names = [ "Josh", "John", "Jawn" ];

// Suppose we only wanted the first element on its own variable
// and the rest as a string representation of the remaining array elements
const [ josh, ...names ] = names;
```
you can use the syntax even on function parameters:
```javascript
const person = {
    name: "Josh",
    age: 12,
    test: "waw"
};

const printPerson = ({ name, age }) => {
    console.log(`${name} is ${age} years old`);
}

printPerson(person);
```

although such syntax can also be used with nested object properties, I kind of oppose on doing so. It does make our lives easier since we would be able to do it in one line, but I feel like it adds too much complexity on a single line and makes your code less readable. **I prefer using this on non-nested object properties**.


## The Object Oriented (OO) approach
When I first started with JavaScript, it felt so different that it didn't have some functionalities Java had. I'm talking about interfaces here, which allows us to focus our code and rely on interfaces instead of the implementation themselves, giving us the freedom to switch concretes at any time. 

I also tried implementing the `Builder` pattern when I first started out JS to represent a database entity. It was 'okay' that I had to do two files: one for the entity and one for the builder class. However, the absence of access modifiers, **especially private** felt like I was a naked man on the middle of a highway. Not much of encapsulation is provided to JavaScript variables, unless you do workarounds.

The different scoping of `this` was also problematic to me, as I was accustomed to the lexical based scoping of Java and other programming languages.

### Classes

## NodeJS require V.S ES6 import/export
It is good to note that currently, there are no browser engines that natively implements `import/export` from the ES6 standard.

From a [stackoverflow thread](https://stackoverflow.com/a/31367852), `Felix Kling` pointed out that
>Babel converts import and export declaration to CommonJS (require/module.exports) by default. So using ES6 module syntax, you will be using CommonJS under the hood if you run the code in Node. There are technical differences between CommonJS and ES6 modules, e.g. CommonJS allows you to load modules dynamically. ES6 doesn't allow this.

![Common JS vs ES6](https://i.stack.imgur.com/5WgFJ.png)

Thanks to `prosti` of stackoverflow for this illustration.



 

## Coding Conventions
Majority of the conventions specified in here are from airbnb's [repository](https://github.com/airbnb/javascript). It's a good collection of JS coding conventions. It just so happened that there's too many of them, and I'm only gonna be cherry picking those I think will be useful in the long run.

1. Symbols cannot be faithfully polyfilled, so they should not be used when targeting browsers/environments that don’t support them natively. As of the time of this writing [IE still does not support symbols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol#Browser_compatibility).

2. When you access a complex type you work on a reference to its value. Complex types are of the following: `object, array, and function`

3. Prefer using `const` and `let` instead of `var`. When you do not expect references and values to change, use `const`. Else, make use of `let`. Both of these keywords are **block scoped**. (Just how a regular variable in Java works)

4. Use the literal syntax for creating objects and arrays. `{}` for objects and `[]` for arrays since there is an alternative approach of instantiating ones through their constructor. E.g: `new Object()`

5. Use computed property names when having dynamic object properties.

```javascript
const imDynamic = (myValue) => {
    return myValue + "huluh";
}

const goodApproach = {
    greeting: "hello",
    [imDynamic("hello")]: "hello again"
};
```

6. Use object and property shorthand.

7. Only quote properties that are invalid identifiers. One example would be key values on an object, which allows them to be specified with or without quotes.

8. Use `Array.push` instead of direct assignment to add items to an array.

9. Use array spreads `...` to copy arrays.

10. Use return statements in array method callbacks.

11. Use object destructuring when accessing and using multiple properties of an object. This prevents you from creating temporary references to those properties.

```javascript
const user = {
    name: "Joshua",
    age: "12",
    favoriteFood: "pizza"
};

const appendNameAndAge = ({name, age}) => {
    return `${name}, ${age}`;
};
```

12. Use array destructing in cases you would like to get multiple items from an array.

```javascript
const myArray = [1, 2, 3, 4, 5];
const [firstElement, secondElement] = myArray;
console.log(firstElement); // outputs 1
console.log(secondElement); // outputs 2
```

13. Use object destructuring to return multiple values instead of array destructuring. Unlike its array counterpart, object destructuring doesn't rely on parameter order.

```javascript
const returnMultipleProperties = (user) => {
    return { name, age, favoriteFood };
};

const { age, name } = returnMultipleProperties(user);
```

15. Long Strings divided into multiple lines makes it harder to search for references with a text editor or IDE. When trying to build a string with variables, make use of template literals.

16. **Do not ever use eval with strings.** Even the [mozilla reference page](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval) says so. Same goes for invoking the `Function` constructor.

17. Prefer using the rest syntax `...` for multiple arguments on a function instead of using `arguments`

18. Use default parameter syntax instead of mutating parameter values.

19. Avoid side effects with default parameters.

20. Put default parameters last.

21. Use `extends` for inheritance.

22. Methods can also return `this`, just like in Java.

23. Avoid useless constructors. JS provides a default one, same with Java.

24. Do not use wildcard imports.

25. Only import from a path in a **single place**.

26. Avoid exporting mutable values.

27. In modules with a single export, prefer `default` over named export.

28. Place all of your imports above non-import statements.

30. Multiline imports should be indented like a multiline array for readability.

31. Prefer the functional approach and avoid iterators. Use map() / every() / filter() / find() / findIndex() / reduce() / some() / ... to iterate over arrays, and Object.keys() / Object.values() / Object.entries() to produce arrays so you can iterate over objects. This enforces **immutability**.

32. Assign variables where you need them, but place them in a reasonable place.

33. Don't chain variable assignments. Uncle Bob's gonna be mad at you for doing such.

34. Prefer using `===` and `!==` over `==` `!=`

35. `if` in Javascript has a little difference on how it works with other languages. From mozilla docs:
> In JavaScript, a truthy value is a value that is considered true when encountered in a Boolean context. All values are truthy unless they are defined as falsy (i.e., except for false, 0, 0n, "", null, undefined, and NaN). 
Reference: https://developer.mozilla.org/en-US/docs/Glossary/Truthy


```javascript
// All evaluates to true
if (true)
if ({})
if ([])
if (42)
if ("0")
if ("false")
if (new Date())
if (-42)
if (12n)
if (3.14)
if (-3.14)
if (Infinity)
if (-Infinity)
```

36. Prefer single line ternaries and avoid having nested ones in a single line.

37. Use braces with all multiline block statements. This allows you to easily add extra line of statements in the future.

38. Use `/** ... */` for multiline comments and `// ..` for single ones.

39. Add a first initial space on comments for readability.

40. Make it a habit to prefix comments with `FIXME` or `TODO` if there is some sort of work that needs to be done. Text editors and IDEs can usually group these comments, providing easier navigation of what else needs to be done.

41. Additional trailing comma helps with git diffs.

42. Make use of semicolons! Adds a bit of readability since semicolons are usually an indicator for the end of statement in other programming languages. Also,
>Why? When JavaScript encounters a line break without a semicolon, it uses a set of rules called Automatic Semicolon Insertion to determine whether or not it should regard that line break as the end of a statement, and (as the name implies) place a semicolon into your code before the line break if it thinks so. ASI contains a few eccentric behaviors, though, and your code will break if JavaScript misinterprets your line break. These rules will become more complicated as new features become a part of JavaScript. Explicitly terminating your statements and configuring your linter to catch missing semicolons will help prevent you from encountering issues.

43. Shorhand constructors for parsing variable types to another. `String(1)`, `Boolean("true")`, etc.

44. Camel case for variables and function names. Pascal case for classes. Guess there's a reason why this stuff was called **Java**script. ;)

45. A base file name should match its default export. Adds readability and prevents confusion.

I have omitted those related with space and tab formatting, as those may differ and be agreed on a certain project. We always have linters to rescue on that anyways. Some other items that I have omitted may have been combined to other items ; or I might have found them to be really subjective or not useful at all.

I suggest taking a look of `Clean Code` by Uncle Bob if you want to read more on coding conventions that are language agnostic (e.g: descriptive variable names, etc.). 

## ES6 Compatibility Table
Check out kangax's repository [here](https://github.com/kangax/compat-table).