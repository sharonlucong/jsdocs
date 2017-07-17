### Common use of ES6
- Constants
```ts
const PI = xxxx;
```
**equivalent to ES5**
```ts
Object.defineProperty(typeof global === "object" ? global : window, "PI", {
    value:        3.141593,
    enumerable:   true,
    writable:     false,
    configurable: false
})
```
##### Enumerable properties show up in for...in loops unless the property's name is a Symbol

- Scopes
block-level instead of function-level

- Arrow Functions
```
1. no need of return
2. don't worry about `this`
```

- Extended Parameters handling

default argument value
```ts
function f(x, y=7, z=32) {
  return x+y+z;
}
```

rest parameter: be able to represent a number of arguments as an array
```ts
function f (x, y, ...a) {
  return (x + y) * a.length;
}

f(1, 2, "hello", true, 7) === 9;
```

spread operator: allows an iterable such as an array expression to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected, or an object expression to be expanded in places where zero or more key-value pairs (for object literals) are expected.
```ts
var params = [ "hello", true, 7 ]
var other = [ 1, 2, ...params ] // [ 1, 2, "hello", true, 7 ]

function f (x, y, ...a) {
    return (x + y) * a.length
}
f(1, 2, ...params) === 9

var str = "foo"
var chars = [ ...str ] // [ "f", "o", "o" ]

var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };

var clonedObj = { ...obj1 };
// Object { foo: "bar", x: 42 }

var mergedObj = { ...obj1, ...obj2 };
// Object { foo: "baz", x: 42, y: 13 }
```

- Template Literals
```ts
var customer = { name: "Foo" }
var card = { amount: 7, product: "Bar", unitprice: 42 }
var message = `Hello ${customer.name},
want to buy ${card.amount} ${card.product} for
a total of ${card.amount * card.unitprice} bucks?`
```
- Enhanced Object Properties
```ts
1. obj = {x, y};

2. Computed property names:
let obj = {
  foo: "bar",
  ["baz" + quux()]: 43
};
```
3. generator function: are functions which can be exited and later re-entered.
```ts
obj = {
    foo (a, b) {
        …
    },
    bar (x, y) {
        …
    },
    *quux (x, y) {
        …
    }
};
```

Yield in a generator： calling a generator function doesn't execute its body immediately; an iterator object for the function is returned instead. Whenthe iterator's next() method is called, the generator function's body is executed until the first `yield` expression.
```ts
function* idMaker() {
  var index = 0;
  while (index < 3)
    yield index++;
}

var gen = idMaker();

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // undefined
```

Returen statement in a generator: A return statement in a generator, when executed, will make the generator done.
```ts
function* yieldAndReturn() {
  yield "Y";
  return "R";
  yield "unreachable";
}

var gen = yieldAndReturn()
console.log(gen.next()); // { value: "Y", done: false }
console.log(gen.next()); // { value: "R", done: true }
console.log(gen.next()); // { value: undefined, done: true }
```

- Destructuring assignment

array matching
```ts
var list = [1, 2, 3];
var [a, , b] = list; //a=1, b=3
[b, a] = [a, b]; //b=1, a=3
```

object/array matching
```ts
var obj = {a: 1};
var list = [1];
var {a, b=2} = obj; //a=1, b=2
var [x, y=2] = list; //x=1, y=2
```

Fail-Soft Destructuring: optionally with defaults
```ts
var list = [ 7, 42 ]
var [ a = 1, b = 2, c = 3, d ] = list
a === 7
b === 42
c === 3
d === undefined
```

Rename:
```ts
/*
wes: {
  links: {
    social: {
      twitter: 'https://twitter.com/wesbos',
      facebook: 'https://facebook.com/wesbos.developer',
    }
  }
}
*/

const { twitter: tweet, facebook: fb } = wes.links.social; // tweet === wes.links.social.twitter; fb === wes.links.social.facebook
```

- Module
```
//lib/foo.js
export function foo(x, y) {return x+y;}

//someApp.js
import * as api from "lib/foo"
console.log(api.foo(1,2));

//or
import {foo} from "lib/foo"
```

- Class

class definition
```ts
class Shape {
  constructor (id, x, y) {
    this.id = id;
    this.move(x, y);
  },
  move (x, y) {
    this.x = x;
    this.y = y;
  }
}
```

class inheritance
```ts
class Rectangle extends Shape {
  constructor(id, x, y, width, height) {
    super(id, x, y);
    this.width = width;
    this.height = height;
  }
}
```

basic class access: `super.xxx;`
```ts
class Shape {
    …
    toString () {
        return `Shape(${this.id})`
    }
}
class Rectangle extends Shape {
    constructor (id, x, y, width, height) {
        super(id, x, y)
        …
    }
    toString () {
        return "Rectangle > " + super.toString()
    }
}
class Circle extends Shape {
    constructor (id, x, y, radius) {
        super(id, x, y)
        …
    }
    toString () {
        return "Circle > " + super.toString()
    }
}
```

static members: Static method calls are made directly on the class and are not callable on instances of the class. Static methods are often used to create utility functions.
```ts
class xxx {
  constructor() {

  }

  static xxx() {

  }
}
```

getter and setter:

```ts
class Rectangle {
    constructor (width, height) {
        this._width  = width
        this._height = height
    }
    set width  (width)  { this._width = width               }
    get width  ()       { return this._width                }
    set height (height) { this._height = height             }
    get height ()       { return this._height               }
    get area   ()       { return this._width * this._height }
}

```

- Promise

Definition: A Promise is a proxy for a value not necessarily known when the promise is created. This lets asynchronous methods return values like synchronous methods: instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some point in the future (receiver of the future value)

Syntax: `new Promise( /* executor */ function(resolve, reject) { ... } );`

Executor: A function that is passed with the arguments resolve and reject. The executor function is executed immediately by the Promise implementation, passing resolve and reject functions


```ts
return new Promise((resolve, reject) => {
  //...
});
```

**Deferred Object**: an object that can create a promise and change its state to `resolved` or `rejected`. It is used when you are the producer of the value.

- New Built-In Methods

number truncation
```ts
Math.trunc(42.7); // 42
Math.trunc(0.1); //0
Math.trunc(-0.1); //-0
```

number comparison:  `Number.EPSILON` property represents the smallest positive value
```ts
console.log(0.1 + 0.2 === 0.3) // false
console.log(Math.abs((0.1 + 0.2) - 0.3) < Number.EPSILON) // true
```

number safty checking
```ts
Number.isSafeInteger(42) === true
Number.isSafeInteger(9007199254740992) === false
Number.isSafeInteger(Infinity) === false
```

array element finding
```ts
[1,2,3,4].find(x => x > 3) //4
[1,2,3,4].findIndex(x => x > 3)//3
```

string repeating
```ts
"foo".repeat(2); //"foofoofoo"
```

string searching
```ts
"hello".startsWith("ello", 1) // true
"hello".endsWith("hell", 4)   // true
"hello".includes("ell")       // true
"hello".includes("ell", 1)    // true
"hello".includes("ell", 2)    // false
```


[Reference] (https://webapplog.com/es6/)