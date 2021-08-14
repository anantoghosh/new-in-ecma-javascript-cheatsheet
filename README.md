# What's new in ECMA / JavaScript cheat sheet

## ECMAScript 2022 - 4 Finished proposals

### 1. RegExp Match Indices
[Proposal](https://github.com/tc39/proposal-regexp-match-indices) • [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) • [v8.dev](https://v8.dev/features/regexp-match-indices)  
Using the regex `d` flag, additionally return the start and end indices for individual capture groups on regex matches.

```js
/(?<x>a)(?<y>b)/d.exec('ab')
// ['ab', 'a', 'b']

/(?<x>a)(?<y>b)/d.exec('ab').indices
// [[0, 2], [0, 1], [1, 2]]

/(?<x>a)(?<y>b)/d.exec('ab').indices.groups
// { x: [0, 1], y: [1, 2] }
```
✅ Chrome - Since v90  
✅ Firefox - Since v89  
🟡 Safari - Since v15? (not mentioned in release notes) [technical preview 122](https://webkit.org/blog/11577/release-notes-for-safari-technology-preview-122/)  
✅ Node - Since v16.0.0 (v8 9.0)  
CanIUse - unavailable

### 2. Top-level await
[Proposal](https://github.com/tc39/proposal-top-level-await) • [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await#top_level_await) • [v8.dev](https://v8.dev/features/top-level-await)  
use `await` outside async functions in a module.

```js
await Promise.resolve(console.log('🎉'));
```

✅ [Babel](https://babeljs.io/docs/en/babel-plugin-syntax-top-level-await)  
✅ [Typescript - Since v3.8](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#top-level-await)  
✅ [SWC](https://swc.rs/docs/comparison-babel)  
✅ [Sucrase](https://sucrase.io/#selectedTransforms=imports&compareWithBabel=false&code=await%20Promise.resolve(console.log('%F0%9F%8E%89'))%3B)  
✅ Chrome - Since v89  
✅ Firefox - Since v89  
🟡 Safari - Since v15 [technical preview 122](https://webkit.org/blog/11577/release-notes-for-safari-technology-preview-122/)  
✅ Node - Since v16.4.0 - not in commonjs modules (v8 9.1)  
[CanIUse](https://caniuse.com/mdn-javascript_operators_await_top_level)

### 3. Class Fields
[v8.dev](https://v8.dev/features/class-fields) • [MDN-1](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Public_class_fields) • [MDN-2](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)  
#### a. Class Instance Fields
[Proposal](https://github.com/tc39/proposal-class-fields)  
Declare fields (`this.variable`) outside constructor. Create private fields which
cannot be accessed from outside the class.

```js
class Fields {
  x = 0;
  #y = 0;

  constructor() {
    this.x = 0;
    this.#y = 1;
  }
}

const obj = new Fields();
console.log(obj.x); // 0
console.log(obj.#y); // error
```

#### b. Private instance methods and accessors
[Proposal](https://github.com/tc39/proposal-private-methods)  
Add private methods and accessors (getter/setters).

```js
class Example {
  #xValue = 0;

  get #x() { 
    return #xValue;
  }

  set #x(value) {
    this.#xValue = value;
  }

  #add() {
    this.#x = this.#x + 2;
  }

  constructor() {
    this.#add();
  }
}

```

#### c. Static class fields and private static methods
[Proposal](https://github.com/tc39/proposal-static-class-features)  

```js
class StaticMethodCall {
  static #privateStaticProperty = 'private static property';
  static staticProperty = 'static property';

  static staticMethod() {
  }
  static #privateStaticMethod() {
  }
}
StaticMethodCall.staticMethod();
```

### 4. Ergonomic brand checks for Private Fields
[Proposal](https://github.com/tc39/proposal-private-fields-in-in) • [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in#private_fields_and_methods) • [v8.dev](https://v8.dev/features/private-brand-checks)  
Check if a private field exists in an object using the `in` operator.

```js
class C {
  #brand;

  #method() {}

  get #getter() {}

  static isC(obj) {
    return #brand in obj && #method in obj && #getter in obj;
  }
}
```

✅ [Babel](https://babeljs.io/docs/en/babel-plugin-proposal-private-property-in-object)  
⛔ [Typescript](https://github.com/microsoft/TypeScript/pull/44648)  
⛔ [SWC](https://swc.rs/docs/comparison-babel)  
⛔ [Sucrase](https://github.com/alangpierce/sucrase#transforms)  
✅ Chrome - Since v91  
✅ Firefox - Since v90  
🟡 Safari - Since v15? (not mentioned in release notes) [technical preview 127](https://webkit.org/blog/11736/release-notes-for-safari-technology-preview-127/)  
✅ Node - Since v16.4.0 (v8 9.1)  
CanIUse - Not available

## ECMAScript 2021 - 5 new features

### 1. Numeric separators [new syntax]
[MDN](https://github.com/tc39/proposal-numeric-separator) • [v8.dev](https://v8.dev/features/numeric-separators)  
Allow making numbers more readable by separating it with `_` (underscore)

```js
let budget = 1_000_000_000_000;
```

✅ [Babel](https://babeljs.io/docs/en/babel-plugin-proposal-numeric-separator)  
✅ [Typescript - Since v2.7](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-7.html#numeric-separators)  
✅ [SWC](https://swc.rs/docs/comparison-babel)  
✅ [Sucrase](https://github.com/alangpierce/sucrase#transforms)  
✅ Chrome - Since v75  
✅ Firefox - Since v70  
✅ Safari - Since v13  
✅ Node - Since v12.5.0  
[CanIUse](https://caniuse.com/MDN-javascript_grammar_numeric_separators)

### 2. Logical Assignment Operators [new syntax]
[MDN](https://github.com/tc39/proposal-logical-assignment) • [v8.dev](https://v8.dev/features/logical-assignment)  
Combine Logical Operators and Assignment Expressions

```js
// "Or Or Equals"
a ||= b;
a || (a = b);

// "And And Equals"
a &&= b;
a && (a = b);

// "QQ Equals"
a ??= b;
a ?? (a = b);
```

✅ [Babel](https://babeljs.io/docs/en/babel-plugin-proposal-logical-assignment-operators)  
✅ [Typescript - Since v4.0 ](https://devblogs.microsoft.com/typescript/announcing-typescript-4-0/#short-circuiting-assignment-operators)  
⛔ [SWC](https://swc.rs/docs/comparison-babel)  
⛔ [Sucrase](https://github.com/alangpierce/sucrase#transforms)  
✅ Chrome - Since v85  
✅ Firefox - Since v79  
✅ Safari - Since v14  
✅ [Node - Since v15.0.0](https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V15.md#v8-86---35415)  
[CanIUse](https://caniuse.com/?search=Logical%20Assignment%20Operators)

### 3. WeakRefs [new object]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakRef) • [v8.dev](https://v8.dev/features/weak-references)  
A WeakRef object lets you hold a weak reference to another object, without preventing that object from getting garbage-collected.  
Related: [FinalizationRegistry](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/FinalizationRegistry)

```js
const ref = new WeakRef(someObject);
// Try to reference the original object
const obj = ref.deref();
if (obj) {
  console.log('The obj is available.');
} else {
  console.log('The obj has been removed.');
}
```

✅ Chrome - Since v84  
✅ Firefox - Since v79  
✅ Safari - Since v14 (iOS Safari v14.7)  
✅ [Node - Since v14.6.0](https://v8.dev/blog/v8-release-84#weak-references-and-finalizers)  
[CanIUse](https://caniuse.com/MDN-javascript_builtins_weakref)  
_Cannot be polyfilled_

### 4. Promise.any [new method]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any) • [v8.dev](https://v8.dev/features/promise-combinators)  
Takes a list of Promises (or iterable) and as soon as one of the promises fulfills(resolves), returns a single promise that resolves with the value from that promise. If no promise fulfills (if all of the given promises are rejected), then the returned promise is rejected with an AggregateError.

```js
const promise1 = Promise.reject(0);
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'quick'));
const promise3 = new Promise((resolve) => setTimeout(resolve, 500, 'slow'));

const promises = [promise1, promise2, promise3];

Promise.any(promises).then((value) => console.log(value));

// output: "quick"
```

✅ Chrome - Since v89  
✅ Firefox - Since v86  
✅ Safari - Since v14  
✅ [Node - Since v15.0.0](https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V15.md#v8-86---35415)  
[CanIUse](https://caniuse.com/?search=promise.any)  
[Polyfill](https://github.com/zloirock/core-js#ecmascript-promise)

### 5. String.prototype.replaceAll [new method]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll) • [v8.dev](https://v8.dev/features/string-replaceall)  
Replace all instances of a substring (literal or regexp) in a string

```js
const queryString = 'q=query+string+parameters';
const withSpaces = queryString.replaceAll('+', ' ');
```

✅ Chrome - Since v85  
✅ Firefox - Since v77  
✅ Safari - Since v13.1  
✅ [Node - Since v15.0.0](https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V15.md#v8-86---35415)  
[CanIUse](https://caniuse.com/MDN-javascript_builtins_string_replaceall)  
[Polyfill](https://github.com/zloirock/core-js#stringreplaceall)

## ECMAScript 2020 - 9 new features

### 1. import.meta [new object]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import.meta) • [v8.dev](https://v8.dev/features/modules#import-meta)  
The `import.meta` is a special object which contains metadata of the currently running JavaScript module.  
`import` is not an object, only `import.meta` is an object. This object is mutable and can be used to store arbitrary information.  
[Example usage in deno.](https://deno.land/manual/examples/module_metadata)  
[Example usage in vite.](https://vitejs.dev/guide/env-and-mode.html#env-variables)  

```js
// Load a module
<script type="module" src="my-module.js"></script>

// my-module.js
console.log(import.meta); // { url: "<url>" }
```

✅ Chrome - Since v64  
✅ Firefox - Since v62  
✅ Safari - Since v11.1 (iOS Safari v12)  
✅ Node - Since v10.4.0  
[CanIUse](https://caniuse.com/MDN-javascript_statements_import_meta)  

### 2. Nullish coalescing Operator [new syntax]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator) • [v8.dev](https://v8.dev/features/nullish-coalescing)  
The nullish coalescing operator `??` is a logical operator that returns its right-hand side operand when its left-hand side operand is null or undefined, and otherwise returns its left-hand side operand.

```js
let x = foo ?? bar();

// Which is equivalent to
let x = (foo !== null && foo !== undefined) ? foo : bar();
```

✅ [Babel](https://babeljs.io/docs/en/babel-plugin-proposal-nullish-coalescing-operator)  
✅ [Typescript - Since v3.7 ](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#nullish-coalescing)  
✅ [SWC](https://swc.rs/docs/comparison-babel)  
✅ [Sucrase](https://github.com/alangpierce/sucrase#transforms)  
✅ Chrome - Since v80  
✅ Firefox - Since v72  
✅ Safari - Since v13.1 (iOS Safari v13.7)  
✅ Node - Since v14.0.0  
[CanIUse](https://caniuse.com/MDN-javascript_operators_nullish_coalescing)  

### 3. Optional Chaining [new syntax]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining) • [v8.dev](https://v8.dev/features/optional-chaining)  
The `?.` operator functions similarly to the `.` chaining operator, except that instead of causing an error if a reference is nullish (null or undefined), the expression short-circuits with a return value of undefined.

```js
let x = foo?.bar.baz();

// Which is equivalent to
let x = (foo === null || foo === undefined) ? undefined : foo.bar.baz();

// Calling an optional method
myForm.checkValidity?.()
```

✅ [Babel](https://babeljs.io/docs/en/babel-plugin-proposal-nullish-coalescing-operator)  
✅ [Typescript - Since v3.7 ](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-7.html#optional-chaining)  
✅ [SWC](https://swc.rs/docs/comparison-babel)  
✅ [Sucrase](https://github.com/alangpierce/sucrase#transforms)  
✅ Chrome - Since v80  
✅ Firefox - Since v72  
✅ Safari - Since v13.1 (iOS Safari v13.7)  
✅ Node - Since v14.0.0  
[CanIUse](https://caniuse.com/MDN-javascript_operators_optional_chaining)  

### 4. for-in mechanics [new behavior]
[Proposal](https://github.com/tc39/proposal-for-in-order)  
Standardize the order of for-in loops.

### 5. globalThis [new object]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis) • [v8.dev](https://v8.dev/features/globalthis)  
Standard way to access the global object in all JavaScript environments (window in Browser, global in Node).

```js
function canMakeHTTPRequest() {
  return typeof globalThis.XMLHttpRequest === 'function';
}

```

✅ [Typescript lib - Since v3.4](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#type-checking-for-globalthis)  
✅ Chrome - Since v71  
✅ Firefox - Since v65  
✅ Safari - Since v12.1 (iOS Safari v12.2)  
✅ Node - Since v12.0.0  
[Polyfill](https://github.com/zloirock/core-js#globalthis)  
[CanIUse](https://caniuse.com/MDN-javascript_builtins_globalthis)  

### 6. Promise.allSettled [new method]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled) • [v8.dev](https://v8.dev/features/promise-combinators)  
`Promise.allSettled` is a new Promise method that returns a Promise that is fulfilled when all of the input promises are fulfilled or rejected.

```js
const promises = [ fetch('index.html'), fetch('https://does-not-exist/') ];
const results = await Promise.allSettled(promises);
const successfulPromises = results.filter(p => p.status === 'fulfilled');
```

✅ [Typescript lib - Since v3.8](https://github.com/microsoft/TypeScript/pull/34065)  
✅ Chrome - Since v76  
✅ Firefox - Since v71  
✅ Safari - Since v13  
✅ Node - Since v12.0.0  
[Polyfill](https://github.com/zloirock/core-js#ecmascript-promise)  
[CanIUse](https://caniuse.com/MDN-javascript_builtins_promise_allsettled)  

### 7. BigInt [new object]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) • [v8.dev](https://v8.dev/features/bigint)  
The `BigInt` type is a new numeric primitive in ECMAScript, which is a signed integer type.  
BigInt would dynamically resize memory to fit the actual value.  
The maximum size of BigInt is unspecified and left to the implementation.  
[https://stackoverflow.com/a/54298760](https://stackoverflow.com/a/54298760)  
[https://github.com/tc39/proposal-bigint/issues/174](https://github.com/tc39/proposal-bigint/issues/174)  
[https://v8.dev/blog/bigint](https://v8.dev/blog/bigint)
```js
const huge = BigInt(9007199254740991)
// 9007199254740991n
// BigInt numbers end with a "n".
```

✅ [Typescript lib - Since v3.8](https://github.com/microsoft/TypeScript/pull/34065)  
✅ Chrome - Since v67  
✅ Firefox - Since v68  
✅ Safari - Since v14 (iOS Safari v14.4)   
✅ Node - Since v10.4.0  
[CanIUse](https://caniuse.com/bigint)  
_Cannot be polyfilled_


### 8. Dynamic import - `import()` [new method]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#dynamic_imports) • [v8.dev](https://v8.dev/features/dynamic-import)  
Dynamic imports allows you to import modules at run-time.

```js
import('/modules/my-module.js')
      .then(module => {
        module.loadPageInto(main);
      })
      .catch(err => {
        main.textContent = err.message;
      });
```
✅ [Babel](https://babeljs.io/docs/en/babel-plugin-syntax-dynamic-import)  
✅ [Typescript - Since v2.4](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-4.html)  
✅ [SWC](https://swc.rs/docs/comparison-babel)  
✅ [Sucrase](https://github.com/alangpierce/sucrase#transforms)  
✅ Chrome - Since v63  
✅ Firefox - Since v67  
✅ Safari - Since v11.1 (iOS Safari v11.3)   
✅ Node - Since v13.2.0, [later enabled in v12.17.0](https://nodejs.org/en/blog/release/v12.17.0/)  
[CanIUse](https://caniuse.com/es6-module-dynamic-import)  

### 9. String.prototype.matchAll [new method]
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll) • [v8.dev](https://v8.dev/features/string-matchall)  
`matchAll` returns a RegExpStringIterator of matches matching a string or regex also with capture groups.

```js
const regexp = /t(e)(st(\d?))/g;
const str = 'test1test2';

const match = str.matchAll(regexp);

for (const m of str.matchAll(regexp)) {
  console.log(m);
}
// ["test1", "e", "st1", "1"]
// ["test2", "e", "st2", "2"]
```

✅ [Typescript lib - Since v3.8](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#es2020-for-target-and-module)  
✅ Chrome - Since v63  
✅ Firefox - Since v67  
✅ Safari - Since v11.1 (iOS Safari v11.3)   
✅ Node - Since v13.2.0, [later enabled in v12.17.0](https://nodejs.org/en/blog/release/v12.17.0/)  
[CanIUse](https://caniuse.com/mdn-javascript_builtins_string_matchall)  

## ECMAScript 2019 - 8 new features

### 1. Array.prototype.{flat,flatMap}
[Proposal](https://github.com/tc39/proposal-flatMap) • [MDN-flat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) • [MDN-flatMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap) • [v8.dev](https://v8.dev/features/array-flat-flatmap)  
`flat` - Create a new array with sub-array elements concatenated into it recursively up to the specified depth.  
`flatMap` - Map over and flatten an array (to 1 level). Equivalent to running `Array.prototype.map` then `Array.prototype.flat` with depth 1.

```js
const arr2 = [0, 1, 2, [[[3, 4]]]];

console.log(arr2.flat(3));
// [0, 1, 2, 3, 4]

let arr = [[1, 3], [4, 6], [7, 9], [10, 12]];

console.log(arr.flatMap(x => [x[0], (x[0] + x[1]) / 2, x[1]]));
// [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 ]
```
[CanIUse](https://caniuse.com/array-flat)

### 2. String.prototype.{trimStart,trimEnd}
[Proposal](https://github.com/tc39/proposal-string-left-right-trim) • [MDN-trimStart](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimStart) • [MDN-trimEnd](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd) • [v8.dev](https://v8.dev/features/string-trimming)  

```js
const string = '  hello world  ';
string.trimStart();
// 'hello world  '

string.trimEnd();
// '  hello world'
```

[CanIUse - trimStart](https://caniuse.com/mdn-javascript_builtins_string_trimstart)  
[CanIUse - trimEnd](https://caniuse.com/mdn-javascript_builtins_string_trimend)

### 3. Well-formed JSON.stringify
[Proposal](https://github.com/tc39/proposal-well-formed-stringify) • [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#well-formed_json.stringify) • [v8.dev](https://v8.dev/features/well-formed-json-stringify)  
Stringify lone surrogates i.e. any code point from U+D800 to U+DFFF, using Unicode escape sequences.
Before this change JSON.stringify would output lone surrogates if the input contained any lone surrogates; such strings could not be encoded in valid UTF-8 or UTF-16:

```js
// Before
JSON.stringify("\uD800"); // '"�"'

// After
JSON.stringify("\uD800"); // '"\\ud800"'

```
### 4. Object.fromEntries
[Proposal](https://github.com/tc39/proposal-object-from-entries) • [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries) • [v8.dev](https://v8.dev/features/object-fromentries)  
It accepts an iterable of key-value pairs and returns a new object whose own keys and corresponding values are given by those pairs.

```js
obj = Object.fromEntries([['a', 0], ['b', 1]]);
// { a: 0, b: 1 }
```
[CanIUse](https://caniuse.com/mdn-javascript_builtins_object_fromentries)

### 5. Function.prototype.toString revision
[Proposal](https://tc39.es/Function-prototype-toString-revision/) • [ExploringJS](https://exploringjs.com/es2018-es2019/ch_function-prototype-tostring-revision.html)  


### 6. Symbol.prototype.description
[Proposal](https://tc39.es/proposal-Symbol-description/) • [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/description) • [v8.dev](https://v8.dev/features/symbol-description)  

```js
const symbol = Symbol('foo');
symbol.description;
// 'foo'
```
[CanIUse](https://caniuse.com/symbol-description)

### 7. JSON superset
[Proposal](https://github.com/tc39/proposal-json-superset) • [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/superset) • [v8.dev](https://v8.dev/features/subsume-json)  
JSON becomes a syntactic subset of ECMAScript. Before this, ECMAScript string literals couldn’t contain unescaped U+2028 LINE SEPARATOR and U+2029 PARAGRAPH SEPARATOR characters which JSON could have.  

[CanIUse](https://caniuse.com/mdn-javascript_builtins_json_json_superset)

### 8. Optional catch binding
[Proposal](https://github.com/tc39/proposal-optional-catch-binding) • [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) • [v8.dev](https://v8.dev/features/optional-catch-binding)

```js
try {
  doSomethingThatMightThrow();
} catch { // catch parameter is now optional
  handleException();
}
```
[CanIUse](https://caniuse.com/mdn-javascript_statements_try_catch_optional_catch_binding)

## ECMAScript 2018 - 8 new features

### 1. Asynchronous Iteration
[Proposal](https://github.com/tc39/proposal-async-iteration) • [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for-await...of) • [v8.dev](https://v8.dev/features/async-iteration)
### 2. Promise.prototype.finally

### 3. RegExp Unicode Property Escapes

### 4. RegExp Lookbehind Assertions

### 5. Rest/Spread Properties

### 6. RegExp named capture groups

### 7. `s` (`dotAll`) flag for regular expressions

### 8. Lifting template literal restriction
[Proposal]() • [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#Lifting_template_literal_restrictions) • [v8.dev](https://v8.dev/features/lifted-template-literals)

## ECMAScript 2017 - 6 new features

### 1. Shared memory and atomics

### 2. Async functions

### 3. Trailing commas in function parameter lists and calls

### 4. Object.getOwnPropertyDescriptors

### 5. String padding

### 6. Object.values/Object.entries

## ECMAScript 2016 - 2 new features

### 1. Exponentiation operator

### 2. Array.prototype.includes
