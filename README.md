# new-in-ecma-javascript-cheatsheet

## ECMAScript 2021

### [Numeric separators [new syntax]](https://github.com/tc39/proposal-numeric-separator)

**Allow making numbers more readable by separating it with `_` (underscore)**

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

### [Logical Assignment Operators [new syntax]](https://github.com/tc39/proposal-logical-assignment)

**Combine Logical Operators and Assignment Expressions**

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
https://caniuse.com/?search=Logical%20Assignment%20Operators

### [WeakRefs [new object]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakRef)

**A WeakRef object lets you hold a weak reference to another object, without preventing that object from getting garbage-collected.**  
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
⛔ Safari  
✅ [Node - Since v14.6.0](https://v8.dev/blog/v8-release-84#weak-references-and-finalizers)  
[CanIUse](https://caniuse.com/mdn-javascript_builtins_weakref)  
_Cannot be polyfilled_  

### [Promise.any [new method]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)

**Takes a list of Promises (or iterable) and as soon as one of the promises fulfills(resolves), returns a single promise that resolves with the value from that promise. If no promise fulfills (if all of the given promises are rejected), then the returned promise is rejected with an AggregateError.**

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
