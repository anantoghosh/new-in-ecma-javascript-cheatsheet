# What's new in ECMA / JavaScript cheat sheet

## ECMAScript 2021 - 5 new features

### [1. Numeric separators [new syntax]](https://github.com/tc39/proposal-numeric-separator)

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
[CanIUse](https://caniuse.com/mdn-javascript_grammar_numeric_separators)

### [2. Logical Assignment Operators [new syntax]](https://github.com/tc39/proposal-logical-assignment)

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

### [3. WeakRefs [new object]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakRef)

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
[CanIUse](https://caniuse.com/mdn-javascript_builtins_weakref)  
_Cannot be polyfilled_

### [4. Promise.any [new method]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)

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

### [5. String.prototype.replaceAll [new method]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)

Replace all instances of a substring (literal or regexp) in a string

```js
const queryString = 'q=query+string+parameters';
const withSpaces = queryString.replaceAll('+', ' ');
```

✅ Chrome - Since v85  
✅ Firefox - Since v77  
✅ Safari - Since v13.1  
✅ [Node - Since v15.0.0](https://github.com/nodejs/node/blob/master/doc/changelogs/CHANGELOG_V15.md#v8-86---35415)  
[CanIUse](https://caniuse.com/mdn-javascript_builtins_string_replaceall)  
[Polyfill](https://github.com/zloirock/core-js#stringreplaceall)

## ECMAScript 2020 - 9 new features

### [1. import.meta [new object]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import.meta)
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
[CanIUse](https://caniuse.com/mdn-javascript_statements_import_meta)  

### [2. Nullish coalescing Operator [new syntax]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)
The nullish coalescing operator (??) is a logical operator that returns its right-hand side operand when its left-hand side operand is null or undefined, and otherwise returns its left-hand side operand.

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
[CanIUse](https://caniuse.com/mdn-javascript_operators_nullish_coalescing)  

### [Optional Chaining [new syntax]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)
The ?. operator functions similarly to the . chaining operator, except that instead of causing an error if a reference is nullish (null or undefined), the expression short-circuits with a return value of undefined.
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
[CanIUse](https://caniuse.com/mdn-javascript_operators_optional_chaining)  

### [for-in mechanics [new behavior]](https://github.com/tc39/proposal-for-in-order)
Standardize the order of for-in loops.

### [globalThis [new object]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis)
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
[CanIUse](https://caniuse.com/mdn-javascript_builtins_globalthis)  

### [Promise.allSettled [new method]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)
Promise.allSettled is a new Promise method that returns a Promise that is fulfilled when all of the input promises are fulfilled or rejected.

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
[CanIUse](https://caniuse.com/mdn-javascript_builtins_promise_allsettled)  

### [BigInt [new object]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)

The BigInt type is a new numeric primitive in ECMAScript, which is a signed integer type.  
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


### [import() [new method]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#dynamic_imports)
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

### [String.prototype.matchAll [new method]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)
`matchAll` returns a array of matches matching a string or regex.

```js
const regexp = /t(e)(st(\d?))/g;
const str = 'test1test2';

str.matchAll(regexp)
```

✅ [Typescript lib - Since v3.8](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#es2020-for-target-and-module)  
✅ Chrome - Since v63  
✅ Firefox - Since v67  
✅ Safari - Since v11.1 (iOS Safari v11.3)   
✅ Node - Since v13.2.0, [later enabled in v12.17.0](https://nodejs.org/en/blog/release/v12.17.0/)  
[CanIUse](https://caniuse.com/mdn-javascript_builtins_string_matchall)  

## ECMAScript 2019 - 8 new features

### Array.prototype.{flat,flatMap}

### String.prototype.{trimStart,trimEnd}

### Well-formed JSON.stringify

### Object.fromEntries

### Function.prototype.toString revision

### Symbol.prototype.description

### JSON superset

### Optional catch binding

## ECMAScript 2018 - 8 new features

### Asynchronous Iteration

### Promise.prototype.finally

### RegExp Unicode Property Escapes

### RegExp Lookbehind Assertions

### Rest/Spread Properties

### RegExp named capture groups

### s (dotAll) flag for regular expressions

### Lifting template literal restriction

## ECMAScript 2017 - 6 new features

### Shared memory and atomics

### Async functions

### Trailing commas in function parameter lists and calls

### Object.getOwnPropertyDescriptors

### String padding

### Object.values/Object.entries

## ECMAScript 2016 - 2 new features

### Exponentiation operator

### Array.prototype.includes
