### TypeScript

#### Advantages
1. Due to static typing, code is more predictable, easier for debugging.

2. TS has a compilation step to JS that catches all kinds of errors before runtime and breaking something

### Installation
```
npm install -g typescript
```

### Compiling
1. terminal: `tsc xx.ts`

```
// for multiple files
tsc xx.ts xxx.ts

// compiles all .ts files in the current folder. (not recursively)
tsc *.ts

// use --watch to automatically compile a ts file
tsc main.ts --watch
```
For more advanced use, we can create a tsconfig.json file with build settings.

### Basicså
1. Generics
```ts
// The <T> after the function name symbolizes that it's a generic function.
// When we call the function, every instance of T will be replaced with the actual provided type.

// Receives one argument of type T,
// Returns an array of type T.

function genericFunc<T>(argument: T): T[] {
  var arrayOfT: T[] = [];    // Create empty array of type T.
  arrayOfT.push(argument);   // Push, now arrayOfT = [argument].
  return arrayOfT;
}

var arrayFromString = genericFunc<string>("beep");
console.log(arrayFromString[0]);         // "beep"
console.log(typeof arrayFromString[0])   // String

var arrayFromNumber = genericFunc(42);
console.log(arrayFromNumber[0]);         // 42
console.log(typeof arrayFromNumber[0])   // number
```