# Road2Js
A compilation of useful JS infos I have acquired throughout my journey of diving into JS as a Java developer.

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

14. Use single quotes for Strings.

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

29. Break the line after the trailing comma.

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

37. Use braces with all multiline block statements.

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