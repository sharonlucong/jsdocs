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

Yield in a generator
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

Returen statement in a generator
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