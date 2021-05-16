---
title: JavaScript basics
date: 2020-10-15 23:34:08
tags: [IT, Programming, JavaScript]
categories: [IT, Programming, JavaScript]
---
# Var, Data types & Ops
## Var
- `let`, `const`: [block](https://javascript.info/destructuring-assignment#the-rest-pattern "plain code block to group stmts") scope [..](https://javascript.info/closure) --- [the old "var"]( "fn/global scope; dec hoist/raise, allow redec(latter ignored)") & [IIFE]( "immediately-invoked fn-expr")

## Data types
- **dynamic typed**, [7+1]( "num/bigInt, str, bool, null/undefined, obj/symbol") basic data types
- `typeof` --- `null`: "object", `alert`: "function" [..](https://javascript.info/types#type-typeof)

<!-- more -->

### Number
- int/float & `Infinity`, `-Infinity`, `NaN` [..](https://javascript.info/types#number)
	- 64-bit --- Imprecise calc & BigInt [..](https://javascript.info/number)
	- repre: `e`, hex/bin/octal --- `parseInt/-float()`, `toString(base)`
	- regular num & NaN test: `isFinite()`, `isNaN()` vs `Object.is()`
	- rounding: multiply-and-divide & `toFixed(n)` 
	- check for int: `Math.round(n) == n` [..](https://javascript.info/testing-mocha)
- auto type convert: unary `+`, math-op/fn & `Number(value)`
- tasks: [Why 6.35.toFixed(1) == 6.3?](https://javascript.info/number#why-6-35-tofixed-1-6-3), [random int from min to max](https://javascript.info/number#a-random-integer-from-min-to-max)

### String
- quotes & `...${...}` [..](https://javascript.info/types#string), special chars: `\n`, `\` [..](https://javascript.info/string#special-characters)
- auto type convert: in output e.g. `alert(..)`, `toString()`: [obj as key](https://javascript.info/object-toprimitive#toprimitive), [JSON.stringify()](https://javascript.info/json#custom-tojson) & `String(..)` [..](https://javascript.info/type-conversions#summary)
- substr `str.slice()`, str compare `str.localeCompare(str2)`, `str.repeat(n)`, charCode `String.fromCodePoint(code)` & `str.codePointAt(i)` (in generate pwd [..](https://javascript.info/generators#generator-composition)) [..](https://javascript.info/string)
- task: [Truncate text](https://javascript.info/string#truncate-the-text)

### Bool
- 6 falsy values [..](https://javascript.info/type-conversions#boolean-conversion)
- auto type convert: logical op `!!`, compareson & `Boolean(..)`

### null & undefined
- `null` : value unknown; 
- `undefined`: init val for unassigned-var & default fn-return
- Nullish coalescing op `??` 

## Ops
- compare diff type (convert to num) & `null == undefined` , `NaN != NaN` & `===`
- `=` [..](https://javascript.info/operators#assignment) & `,` [..](https://javascript.info/operators#comma)

# Function
**Fn creation** [..](https://javascript.info/function-expressions#function-expression-vs-function-declaration)
- **fn dec**: block-level (if strict), def-hoisting (start with `function`, require a name & cannot be called immediately [..](https://javascript.info/var#iife))
- **fn-expr**: easy dynamic def 
- **arrow-fn** (see obj)
- `new Function([...params]: ...str, body: str)`: create fn dynamically at run time (golbal scope) [..](https://javascript.info/new-function) --- `eval(code: str)` [..](https://javascript.info/eval)

**Fn is obj**
Fn: a callable action (instead of data [..](https://javascript.info/function-expressions#callback-functions)) obj [..](https://javascript.info/function-object)
- fn-def: store the fn-obj in same named var, call by `fnName(...args)` --- callback & *anonymous* [..](https://javascript.info/function-expressions#callback-functions)
- Fn built-in props: `name` (str, "" for anonymous), `length` (param #, rest param not count)
- custom prop/helper method: `fnName.xxx = ..`
- tasks: [Set and decrease for counter](https://javascript.info/function-object#set-and-decrease-for-counter), [Sum with an arbitrary amount of brackets](https://javascript.info/function-object#sum-with-an-arbitrary-amount-of-brackets)

**Closure**
- block scope & Lexical Environment (EnvRecord & outer-ref, created on run time) ...
- LE var/fn pre-populate & var "dead zone" [..](https://javascript.info/closure#is-variable-visible)
- access var through **LE chain** (inner-outer) [..](https://javascript.info/closure#step-3-inner-and-outer-lexical-environment) & updated on the LE it lives [..](https://javascript.info/closure#step-4-returning-a-function)

[Closure](https://javascript.info/closure#step-4-returning-a-function): a fn that remembers its outer vars and can access them by e.g. return a fn (or as obj prop) [..](https://javascript.info/closure#nested-functions)
- run outerFn: new outerFn.LE & nestedFn created, `nestedFn.[[Env..]] = outerFn.LE` (set once & forever)
  - so outerFn.LE still reachable & not GC [..](https://javascript.info/closure#garbage-collection)
  - call outerFn mtp times: diff outerFn.LE & nestedFn created (nestedFn independent) [..](https://javascript.info/closure#are-counters-independent)
- run nestedFn: new nestedFn.LE created & `nestedFn.LE -> outerFn.LE` (ref value taken from [[Env..]])
- task: [Counter object](https://javascript.info/closure#counter-object), [Sum with closures](https://javascript.info/closure#sum-with-closures), [Filter through function](https://javascript.info/closure#filter-through-function), [Sort by field](https://javascript.info/closure#sort-by-field), [Army of functions](https://javascript.info/closure#army-of-functions)

# Obj, Arrow Fn, Con-Fn
**obj** --- str/symbol: any type
- key can be reserved word, non-str/symbol key ==> toString [..](https://javascript.info/object#property-names-limitations)

syntax
- key existence test: `in` op & optional chaining syntax `?.`  (check `null/undefined`)
- key iterate: `for..in` loop & prop order ..
- value access: `.` vs `obj["prop"]` [..](https://javascript.info/object#square-brackets)
- prop add/set: `obj.prop = xxx`, delete: `delete obj.prop` [..](https://javascript.info/object#literals-and-properties)
- computed prop, prop value shorthand ..

obj deep cloning: `for..in` & `typeof` [..](https://javascript.info/object-copy#cloning-and-merging-object-assign)

tasks [Check for emptiness](https://javascript.info/object#ordered-like-an-object)

<!-- more -->

## Methods & "this"
method def: fn-expr shorthand || copy from fn [..](https://javascript.info/object-methods#method-examples)

`this` eval at run-time --- [losing "this" problem](https://javascript.info/bind#losing-this "obj method passed around & called in another context")
- call fn with obj ("this" in methods): `this=obj` [..](https://javascript.info/object-methods#this-in-methods)
- call fn without obj (as fn itself) [..](https://javascript.info/object-methods#this-is-not-bound)
	- `this=undefined` if strict & `arr.forEach(fn)` [..](https://javascript.info/arrow-functions#arrow-functions-have-no-this)
	- `this=window` if non-strict & `setTimeout(fn, ms)` [..](https://javascript.info/bind#losing-this), [..](https://javascript.info/call-apply-decorators#delaying-decorator)
- tasks: [Using "this" in object literal](https://javascript.info/object-methods#using-this-in-object-literal), [calculator](https://javascript.info/object-methods#create-a-calculator), [Chaining](https://javascript.info/object-methods#create-a-calculator)
- losing this problem & [reference type](https://javascript.info/reference-type), task: [Explain the value of "this"](https://javascript.info/reference-type#explain-the-value-of-this)

bind "this"
- [fn binding](https://javascript.info/bind#solution-2-bind): `fn.bind(context, [...startingArgs])`
	- `this=context` (bound [hard-fixed](https://javascript.info/bind#bound-function-as-a-method "use pre-bound context value (even bound obj changes later), so cannot re-bound")), partial [..](https://javascript.info/bind#partial-functions), [..](https://javascript.info/bind#going-partial-without-context)
	- tasks: [Bound fn in method](https://javascript.info/bind#bound-function-as-a-method), [re-bound ignored](https://javascript.info/bind#second-bind), [Fn prop lost af bind](https://javascript.info/bind#function-property-after-bind), [method bind context in callbacks](https://javascript.info/bind#fix-a-function-that-loses-this), [Partial & method](https://javascript.info/bind#partial-application-for-login)
- [call forwarding](https://javascript.info/call-apply-decorators#func-apply "Passing all args context to another fn"): `fn.apply(context, args: arrLike)` (prefer) & `fn.call(context, ...args);`
- [fn-borrowing](https://javascript.info/call-apply-decorators#method-borrowing "take a method from an obj and call it in the context of another obj"): `[].join.call(arguments);` in fn-body

## Obj & Fn
### Arrow fn
`(...args) => expr || { ...body } || (obj-litiral)` [..](https://javascript.info/array-methods#map-to-objects)
- no own `this` (cann't run with `new`) 、`super` [..](https://javascript.info/class-inheritance#overriding-a-method) & `arguments` (no own context [..](https://javascript.info/arrow-functions), take from outer LE) [..](https://javascript.info/arrow-functions#summary)

### F: Constructor fn
- set props & methods `this.xxx = ...`
- call by `new` op (execute "internal algorithm")
  - return this ||  another obj (prim-return ignored)
  - Constructor mode test: `new.target`
- singleton obj `new function() { ... }`

# Built-in Data Structure
## Array
- value indexing: `arr[i]`, value iter: `for..of` || `arr.forEach()`
- `arr.length` = greatest index + 1, init as item #, writable
	- manually increase: `undefined` values (`""` in `toString()`)
	- manually decrease: elem lost --- clear arr `arr.length = 0`	
- `delete arr[i]`: value => `undefined` (`length` keep same) [..](https://javascript.info/array-methods#splice)
- `arr.toString()` => `_,_,_...`
- [Object.keys/values/entries(obj)](https://javascript.info/keys-values-entries#object-keys-values-entries) => arr (enum & non-symbol key)
	- task: [Sum the props](https://javascript.info/keys-values-entries#sum-the-properties)

<!-- more  -->
Methods [..](https://javascript.info/array-methods#summary)
- `splice(..)` : [...rmed-elems]
- `filter(fn)`: [...matchedItems], `find(fn)`, `indexOf()`, `includes()`
- `map(fn)` & `reduce(..)`, `sort(..)` & `reverse()`
- `arr.join(glue=',')` & `str.split(delim)` (`('')`: char[])
- `Array.isArray(value)`

**Iterable & array-like**
- **iterable** (e.g. arr & str) can be used in `for..of` [..](https://javascript.info/iterable)
- array-like: obj with **numeric index & length** [..](https://javascript.info/iterable#array-like)
- str both iter & arr-like [..](https://javascript.info/iterable#array-like) --- `str[i]` & `str.length` 
- iter/arr-like => array: `Array.from(obj, ..)`

`...`: **Spread syntax & Rest param**
- Spread syntax: `...iter`: item list (use **for..of** internally) 
	- copy/merge/convert by `[...iter]` / `{...obj}`
- Rest params: `...arr` in fn-def
  - fn can be called with any # of args (only args assoc with param will be count) [..](https://javascript.info/rest-parameters-spread#rest-parameters)
  - the old `arguments` (both iter & arr-like) [..](https://javascript.info/rest-parameters-spread#the-arguments-variable)

**Destructuring assignment syntax**
- Copying iter-items into vars: `let [...vars]/{...obj-props} = arr/obj` (keep origin unmodified)
	- ignore arr-item by extra `,` [..](https://javascript.info/destructuring-assignment#array-destructuring), `:` for prop-var rename [..](https://javascript.info/destructuring-assignment#object-destructuring), `=` for var/obj-prop-var default value (unprovide value in right-side as **undefined**) [..](https://javascript.info/destructuring-assignment#default-values), 
- easy swap: `[guest, admin] = [admin, guest];`
- [nested desructing](https://javascript.info/destructuring-assignment#nested-destructuring) : same structure (no var for the group one)
- [Smart fn params](https://javascript.info/destructuring-assignment#smart-function-parameters): pass obj as fn param
- obj destcructing to existing var: `(`{...}`)` ({} in main code flow as code block) [..](https://javascript.info/destructuring-assignment#the-rest-pattern)

tasks 
- [A maximal subarray](https://javascript.info/array#a-maximal-subarray)
- [Filter range "in place"](https://javascript.info/array-methods#filter-range-in-place)
- [Sort in decreasing order](https://javascript.info/array-methods#sort-in-decreasing-order)
- [Create an extendable calculator](https://javascript.info/array-methods#create-an-extendable-calculator)
- [Sort users by age](https://javascript.info/array-methods#sort-users-by-age)
- [Shuffle an array](https://javascript.info/array-methods#shuffle-an-array)
- [Filter unique array members](https://javascript.info/array-methods#filter-unique-array-members): `Array.from(new Set(arr))`
- [Create keyed object from array](https://javascript.info/array-methods#create-keyed-object-from-array)

## JSON
- Send data: `JSON.stringify(obj, ..)` & `obj.toJSON()`: formatted str (aka *serialized* obj)
- Get data: `JSON.parse(str, ..)`: obj

# Prototypal inheritance
**[[Prototype]] & prototype**
- `[[Prototype]]`: null || obj (aka **prototype**) [..](https://javascript.info/prototype-inheritance), accessor methods & fully obj-clone [..](https://javascript.info/prototype-methods)
- the deprecated `__proto__` prop [..](https://javascript.info/prototype-inheritance#prototype), reside in `Object.prototype` [..](https://javascript.info/prototype-methods#very-plain) (why depr & "Very plain / pure dict" obj [..](https://javascript.info/prototype-methods#very-plain))

**Prototypal inheritance**: prop not exist in the obj -> find from proto (follow `[[Prototype]]` ref chain) [..](https://javascript.info/prototype-inheritance#summary)
- proto only for reading prop, write/delete data prop act on the obj (write may call setter in proto) [..](https://javascript.info/prototype-inheritance#writing-doesn-t-use-prototype)
- proto search opt [..](https://javascript.info/prototype-inheritance#searching-algorithm)
- `this` in proto method: ref to obj bf `.` (method shared but obj state not) [..](https://javascript.info/prototype-inheritance#the-value-of-this)
- inherited prop in `for..in` (if enum) but not in `Object.keys/values()` , diff by `obj.hasOwnProperty(key)` [..](https://javascript.info/prototype-inheritance#for-in-loop)
- task: [add toString() to the "very plain" obj](https://javascript.info/prototype-methods#add-tostring-to-the-dictionary)

**F.protoype**: a "normal" prop of con-fn F, set `[[Proto..]]` for obj from F when `new F()`
- `let obj = new F()`: set `obj.[[Prototype]] = F.prototype` (`{constructor: F}` by default)
- so `obj.constructor === F` & `let obj2 = new obj.constructor()`
	- task: [Create obj with same constructor](https://javascript.info/function-prototype#create-an-object-with-the-same-constructor)
- re-assign **F.protoype** to another obj: prev created obj.[[Prototype]] not affect
- native F.prototypes
	- `Object.prototype`: inherited to all obj, ref to a huge obj with toString and other methods
		- `Object.prototype.__proto__ = null`
	- Built-in obj (e.g. obj, arr, fn, num 5): store data in obj itself, store methods in prototype [..](https://javascript.info/native-prototypes#other-built-in-prototypes) & [..](https://javascript.info/native-prototypes#summary)
- Check prototype chain by `console.dir(obj)` [..](https://javascript.info/native-prototypes#other-built-in-prototypes)
- task: [Add decorating "defer()" to fns](https://javascript.info/native-prototypes#add-the-decorating-defer-to-functions), [the diff btw calls](https://javascript.info/prototype-methods#the-difference-between-calls)

# Class
```javascript
class F {
  [static] field = ..; // access field in method by "this.xxx"

  constructor(..) { /*..*/ } // called when "new"
  
  [static] method() { /*..*/ } 
  [get/set] method(..) { /*..*/ }
  [expr]() { /*..*/ } // computed name
}
```
- bind `this` in method [..](https://javascript.info/class#making-bound-methods-with-class-fields)
```javascript
  xxx = () => {..} // field store in obj itself ("this" = obj)
  this.xxx = this.xxx.bind(this); // bind existing method in constructor 
  ```
- access `static` outside F: `F.xxx` (`F` as 	`this`) [..](https://javascript.info/static-properties-methods#static-properties), [..](https://javascript.info/static-properties-methods)
  - return obj in static method by `new this(..)` (factory method)
- Class Encapsulation [..](https://javascript.info/private-protected-properties-methods)
  - **protected**: `_xxx` (convention) --- `proxy` [..](https://javascript.info/proxy#protected-properties-with-deleteproperty-and-other-traps)
  - **private**: `#xxx` (proposal, cannot access by `this['#xxx']`)
  - **read-only prop**: only getter no setter (set value on con)
- task: [class Clock](https://javascript.info/class#rewrite-to-class)

## Class vs fn
`typeof F`: `"function"` [..](https://javascript.info/class#what-is-a-class)
- `class F { .. }`: **F** (con-fn) & **F.prototype** [..](https://javascript.info/class#what-is-a-class)
  ```javascript
  F: {
	  constructor code,
		
    ...statics, 
    [[Prototype]]: ParentF || Function.prototype 
  }
  ```
	```javascript
	F.prototype: { 
	  ...methods, // non-enum
		
	  constructor: F, 
    [Prototype]]: ParentF.prototype || Object.prototype 
  }
	```
- `new F()`: obj
  ```javascript
  obj: {
    ...fields,
    [[Prototype]]: F.prototype // access to non-static methods
  }
  ```
- class & fn diffs [..](https://javascript.info/class#not-just-a-syntactic-sugar)

## Class inheritance
`extends` set 2 proto refs: [..](https://javascript.info/static-properties-methods#inheritance-of-static-properties-and-methods)
- `F1.[[Prototype]] = F2` (con-fn inheritance, for statics)
- `F1.prototype.[[Prototype]] = F2.protptype` (for methods)

**Member overriding**
- override con/method: `super(..)` & `super.method(..)` [..](https://javascript.info/class-inheritance#overriding-a-method)
	- default child-con: `super(...args)` [..](https://javascript.info/class-inheritance#overriding-constructor)
	- call `super(..)` in child-con bf using `this` [..](https://javascript.info/class-inheritance#overriding-constructor) ([why?](https://javascript.info/class-inheritance#super-internals-homeobject))
		- when `new`: child-con create `this` by parent-con (in regular fn, an empty obj created for `this`) [..](https://javascript.info/class-inheritance#overriding-constructor)
		- method.`[[HomeObject]]` = their class/obj [..](https://javascript.info/class-inheritance#homeobject) (bound forever [..](https://javascript.info/class-inheritance#methods-are-not-free)) & `super` find parent methods in `[[HomeObject]].[[Prototype]]` [..](https://javascript.info/mixins#a-mixin-example)
		- so method with `super` not free (it uses `[[HomeObject]]`) [..](https://javascript.info/class-inheritance#methods-are-not-free)
		- `super` only work for `method()`, not `method: function() { .. }` [..](https://javascript.info/class-inheritance#methods-not-function-properties)
- override field: `field = ..` (parent-con use own field value, not overriden one) [..](https://javascript.info/class-inheritance#overriding-class-fields-a-tricky-note)
- task: [Extended clock](https://javascript.info/class-inheritance#extended-clock), [Class extends Object?](https://javascript.info/static-properties-methods#class-extends-object)

**Extending built-in classes** [..](https://javascript.info/extend-natives)

**Class Type Checking** [..](https://javascript.info/instanceof)
- `obj instanceof F` --- `F[Symbol.hasInstance](obj)` & `F.prototype.isPrototypeOf(obj)`
- `Object.prototype.toString`: `[object xxx]` (xxx: value of `obj[Symbol.toStringTag]`)

## Mixin
A class / obj contains methods for other classes without inheritance
- imp by copying method into prototype `Object.assign(F.prototype, mixin)` (prevent overwrite)
- [EventMixin example](https://javascript.info/mixins#eventmixin)

# Error handling
Error obj: `new Error/Syntax*/Reference*/Type*(msg)` [..](https://javascript.info/try-catch#throw-operator)
- props: `name`(F): `message` e.g. "Ref..Error: xxx is not defined" in alert [..](https://javascript.info/try-catch#error-object)

`try..catch..finally`: **sync** & **run-time** err / exception [..](https://javascript.info/try-catch#the-try-catch-syntax)
- throw err (in try || rethrow in catch): `throw`
- checking err: `instanceof` [..](https://javascript.info/custom-errors#extending-error)
- Global handler: `window.onerror` [..](https://javascript.info/try-catch#global-catch)

custom err/ extending err: `ValidationError` [..](https://javascript.info/custom-errors#extending-error) & `PropertyRequiredError` example [..](https://javascript.info/custom-errors#further-inheritance)
- set name: `this.name = this.constructor.name;`
- wrapping-exception pattern: catch syntax & validation errs and throws ReadError [..](https://javascript.info/custom-errors#wrapping-exceptions)