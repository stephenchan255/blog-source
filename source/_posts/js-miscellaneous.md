---
title: JavaScript miscellaneous
date: 2020-10-22 19:08:31
tags: [IT, Programming, JavaScript]
categories: [IT, Programming, JavaScript]
---

# Chrome console & Debugging
Chrome console [..](https://javascript.info/devtools#google-chrome) & debuging in chrome [..](https://javascript.info/debugging-chrome)
- logging with `console.log(...)`
- **breakpoints** & `debugger` cmd, execution tracking ...

# 'use strict'
To enable ES5
- off by default --- always put `'use script'` on **top** of script/fn-body
  - auto enable for `class` & `module`
  - no way to cancel once enter strict mode
- non-strict: assignment to non-existing var (without declear) ==> create a new global var (for compatibility) [..](https://javascript.info/closure#step-3-inner-and-outer-lexical-environment)

<!-- more -->

# Data types
- `Object` key: str/symbol (keyed values collection)
- `Array` key: int (ordered collection, [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array))
- `Map` key: any type
- `Set`: unique values

## Date & time
- [creation](https://javascript.info/date#creation) & `Date.parse(str)` [..](https://javascript.info/date#date-parse-from-a-string)
- [get/set date components](https://javascript.info/date#access-date-components): `get/set*()`, [auto-correction](https://javascript.info/date#autocorrection)
- work on timestamp
	- `Date.now()`: get timestamp without intermediate Date obj
	- `date.getTime()`: faster than auto-date-to-num-convert (by +/-) [..](https://javascript.info/date#benchmarking)
  - more precise time measure [..](https://javascript.info/date#summary)
- tasks: [Show a weekday](https://javascript.info/date#show-a-weekday) & [European weekday](https://javascript.info/date#european-weekday), [getDateAgo(date, days)](https://javascript.info/date#which-day-of-month-was-many-days-ago), [getLastDayOfMonth(year, month)](https://javascript.info/date#last-day-of-month)

## Map & Set, WeakMap & WeakSet
`Map`: obj with key of any type, key compare: `===` & `NaN === NaN`
- `new Map([ [_,_], [_,_] ..])` & obj <=> map: `new Map(Object.entries(obj))` [..](https://javascript.info/map-set#object-entries-map-from-object) & `Object.fromEntries(map)` [..](https://javascript.info/map-set#object-fromentries-object-from-map)
- iter: `forEach(..)`, `keys/values/entries()` [..](https://javascript.info/map-set#iteration-over-map)

`Set`: unique values
- methods similar to map, get/set() => add(), iter by insertion order [..](https://javascript.info/map-set#summary)
- Filter unique array members: `Array.from(new Set(arr))`[..](https://javascript.info/map-set#filter-unique-array-members)

`WeakMap`/`WeakSet`: obj unreachable by other means -> rmed from memory & WM/WS
- only **obj** as key/items in WM/WS
- methods [..](https://javascript.info/weakmap-weakset#weakmap) 
- WeakMap use cases: [additional data](https://javascript.info/weakmap-weakset#use-case-additional-data), [caching](https://javascript.info/weakmap-weakset#use-case-caching)

# Obj advanced
## Symbol type
unique (even same desc) vs global symbol (1 key 1 symbol)

```javascript
alert(Symbol("id"));    // TypeError (not auto-convert to string)
alert(id.toString()); 	// Symbol(id)
alert(id.description);  // id
```

## Obj prop config
prop descriptor: `value` + `writable`, `enumerable`, `configurable` [..](https://javascript.info/property-descriptors)
- get/set desc: `Object.getOwnPropertyDescriptor()`, `Object.defineProperty()` & clone with desc [..](https://javascript.info/property-descriptors#object-getownpropertydescriptors)
- obj created "the usually way": flags default `true`; flags not supplied in desc defalut `false`
- built-in `toString()` non-enumerable
- Sealing an object globally ...

Accessor `get/set xxx([value]) {..}`, trigger when `xxx` prop read/assign
- desc: `get/set([value]) {..}`, `enumerable`, `configurable`
- smarter g/setter: store value in another internal prop
- age -> birthday example [..](https://javascript.info/property-accessors#using-for-compatibility)

## Key iter
- method differs (all return an arr) [..](https://javascript.info/prototype-methods#summary), [..](https://javascript.info/proxy#iteration-with-ownkeys-and-getownpropertydescriptor)
	- `Reflect.ownKeys(obj)`/`obj.hasOwnProperty(key)`: own
	- `Object.getOwnPropertyNames`: own, non-symbol --- `Object.getOwnPropertySymbols(obj)`: own, symbol
	- `Object.keys/values/entries(obj)`: own, **non-symbol** & `enum`
	- `for..in`: own + **proto**, **non-symbol** & `enum`
	- `Object.assign()`: symbol + non-symbol [..](https://javascript.info/symbol#symbols-are-skipped-by-for-in)

## Global obj
Provide built-in var & fn (can be access directly), e.g. `window` in browser & `global` in node 
- e.g. `window.alert`, `window.innerHeight` & global fn || **var**-var
- make value global: `window.xxx = ...` (props of global-obj)

## Primitive as Obj
provide methods to op prim but keep them fast & lightweight e.g.  `String`, `Number`, `Boolean`, `Symbol` 
- temporary wrapper obj created & destroyed [..](https://javascript.info/primitives-methods#a-primitive-as-an-object) --- manually modify obj wrap => err (strict) || undefined [..](https://javascript.info/primitives-methods#can-i-add-a-string-property)

## Obj to primitive
- [3 hints]( "which type to convert"): `"string"`, `"number"` & `"default"` (no boolean hint: all **obj  => true**)
- 3 methods: must return a prim value [..](https://javascript.info/object-toprimitive#return-types)
  - `[Symbol.toPrimitive](hint)` return obj ==> err
  - built-in `toString()` => "[object Object]", `valueOf()` => obj itself (ignored) [..](https://javascript.info/object-toprimitive#tostring-valueof)

# JS engine internal: Event loop
[Macrotask queue](https://javascript.info/event-loop#event-loop): handle tasks e.g. **current rgl sync script**, **external script** load & exe, **UI/network event handler** (e.g. `mousemove`), setTimeout **callback**
  - tasks come while engine busy --- enqueued to the queue
  - re-render only af task complete --- if current task tasks long time:  **Page Unresponsive** alert
  - enqueue a macrotask:  `setTimeout(f)`
  - use case: [split CUP-heavy task by setTimeout](https://javascript.info/event-loop#use-case-1-splitting-cpu-hungry-tasks) => react other tasks in-btn / [progress indication](https://javascript.info/event-loop#use-case-2-progress-indication) [...](https://javascript.info/event-loop#summary)
  
[Microtask queue](https://javascript.info/microtask-queue) (aka `PromiseJobs` queue): handle async promise handlers in `.then/catch/finally` & `await`
  - handler enqueued when promise settled, run af current code & prev micro finished [..](https://javascript.info/microtask-queue#microtasks-queue)
  - global `unhandledrejection` event: triggered when microtask queue completed but exist "rejected" promise
  - make code exe af `.then/catch/finally`: chained by `.then()`
  - enqueue a microtask --- `queueMicrotask(f)` [..](https://javascript.info/event-loop#macrotasks-and-microtasks)

JS exe-flow based on *event loop*: engine sleep until tasks appear, then exe from the oldest one (FIFO) [..](https://javascript.info/event-loop)
- exe process (no macrotask in-btw microtask --- ensure app-env same) [..](https://javascript.info/event-loop#summary)
  1. **current rgl sync script** (=> **all microtasks** =>  **render**) 
  2. **some macrotask** (=> **all microtasks** =>  **render**)
  3. **another macrotask** 
  4. ...
- [cpu-heavy event & web worker: another thread](https://javascript.info/event-loop#summary)

# Popup win & iframe
[Popup win](https://javascript.info/popup-windows "a sep win with own JS env, safe to open un-trusted 3rd-party site"): `window.open(URL, ..)` [..](https://javascript.info/popup-windows#window-open) --- open a new tab (by default) in browser (or win if sizes provided [..](https://javascript.info/popup-windows#summary))
- win <-> popup: `let popupWin = window.open(..);` [..](https://javascript.info/popup-windows#accessing-popup-from-window), `popupWin.opener` [..](https://javascript.info/popup-windows#accessing-window-from-popup)
- close popup: `win.close()` & `win.closed` (status check) [..](https://javascript.info/popup-windows#closing-a-popup)
- win focus/blur: `win.focus/blur()` & `win.onfocus/onblur` [..](https://javascript.info/popup-windows#focus-blur-on-a-window)
- win scrolling & resizing [...](https://javascript.info/popup-windows#scrolling-and-resizing)

[Embedded win](ttps://javascript.info/cross-window-communication#in-action-iframe "a sep embeded win with own sep `document` and `window` obj"): `<iframe ..>` --- `iframe.contentWindow/contentDocument` [..](https://javascript.info/cross-window-communication#in-action-iframe)
- wrong doc pitfall: `iframe.contentDocument` when `iframe.onload` (wrong for the one immediately af iframe created) [..](https://javascript.info/cross-window-communication#iframe-wrong-document-pitfall)
- `window.frames` (named coll) & win hierarchy (iframe contains iframes) --- check whether iframe by `win === top` [..](https://javascript.info/cross-window-communication#collection-window-frames) 
- iframe `sandbox` attr (exclude certain actions inside iframe) [...](https://javascript.info/cross-window-communication#the-sandbox-iframe-attribute)
- transparent iframe & clickjacking attack (intercept a single click [..](https://javascript.info/clickjacking#summary)) [...](https://javascript.info/clickjacking)

# Bin data, files
meet binary data: files/img (create, upload, download) [..](https://javascript.info/arraybuffer-binary-arrays)

## BufferSource
- `ArrayBuffer(byteLength)`: store raw bin data (byte-seq) --- read data by 2 kinds `ArrayBufferView` [..](https://javascript.info/arraybuffer-binary-arrays#summary)
	- mtp typed arrays [...](https://javascript.info/arraybuffer-binary-arrays#typedarray): choose data format at constructor, e.g. Uint8Array [..](https://javascript.info/arraybuffer-binary-arrays#dataview)  --- "out-of-bounds" problem [..](https://javascript.info/arraybuffer-binary-arrays#out-of-bounds-behavior) & methods ??? [...](https://javascript.info/arraybuffer-binary-arrays#typedarray-methods), task: [Concat typed arrays](https://javascript.info/arraybuffer-binary-arrays#concatenate-typed-arrays)
	- `DataView`: choose data format at method, e.g. `dataView.get/setUnit8(..)` [...](https://javascript.info/arraybuffer-binary-arrays#dataview)
- `BufferSource`: ArrayBuffer + ArrayBufferView [..](https://javascript.info/arraybuffer-binary-arrays#summary)
- str <=> bytes: `textEncoder.encode(str)` (utf-8) & `new TextDecoder(encoding, ..).decode(BufferSource, ..)` [..](https://javascript.info/text-decoder)

## Blob
`Blob` bin data type [...](https://javascript.info/blob): file up/download op & web network request [..](https://javascript.info/blob#summary)
- Blob type `'text/html | text/plain'` → `Content-Type` in network requests 
- Blob → URL (for `<a>/<img>`) [...](https://javascript.info/blob#blob-as-url)
	`<a download="hello.txt">`, `a.href = URL.createObjectURL(blob)` 
	`a.click()`, `URL.revokeObjectURL(a.href)`
- img → Blob (via `<convas>`) [...](https://javascript.info/blob#image-to-blob)

## File & FileReader
- `File`: inherit from `Blob`, get from `input.files[0]` & Drag’n’Drop events [..](https://javascript.info/file)
- `FileReader`: read data from `Blob` via `fr.readAs*(blob)` (async), `load/error` events & `fr.result / error` [..](https://javascript.info/file#filereader)
- sent file over network: `Fetch`/ `XHR` [..](https://javascript.info/file#summary)

# URL object
- `new URL(url: str, [base])`, url components, char encoding [...](https://javascript.info/url)

# Web Components
## Custom HTML elem
- **all-new**: extends `HTMLElement` [..](https://javascript.info/custom-elements) (no semantics [..](https://javascript.info/custom-elements#customized-built-in-elements))
	- 2-step creation: JS class + `customElements.define(tag, js-class)` [...](https://javascript.info/custom-elements)
	- `<time-formatted>` & `Intl.DateTimeFormat(..)` demo ...
- **customized**: extends built-in elem (with semantics) --- 3-step creation (`<button is="my-button">`) [...](https://javascript.info/custom-elements#customized-built-in-elements)
- html tag `<my-element>` / `document.createElement(..)`: create instance of `MyElement` [..](https://javascript.info/custom-elements)
- CSS selector `:not(:defined)`: style "undefined" elems, e.g. custom html-tag bf `customElements.define(..)` [..](https://javascript.info/custom-elements#example-time-formatted)
- HTML parser process parent bf child [...](https://javascript.info/custom-elements#rendering-order)
	- access childElem by `setTimeout(..)` (af html parsing completed)
	- want outer callback trigger af inner elem ready: inner elem dispatch events & outer listen
- task: [Live timer elem](https://javascript.info/custom-elements#live-timer-element)

## Shadow DOM
- Shadow tree (for encapsulation: own id/quary spaces, styling [..](https://javascript.info/shadow-dom#encapsulation), [..](https://javascript.info/shadow-dom#summary)) vs Light tree [...](https://javascript.info/shadow-dom#shadow-tree)
	- create by `elem.attachShadow(..)` => shadow root: `elem.shadowRoot` 
		- `elem.shadowRoot.host`: the elem, shadow tree host [...](https://javascript.info/shadow-dom#shadow-tree)
		- shadow root like elem (`innerHTML` | DOM methods appliable), but cannot have event handler (use its 1st child) [..](https://javascript.info/slots-composition#updating-slots)	
	- if elem has both -> only render shadow --- compose by **slot** [...](https://javascript.info/slots-composition)
		- `<slot name="...">` (shadow) & elem with `slot="..."` (light, render as child elem of `<slot>`)
		- `slotchange` event (when slotted-elems add/rm/replace) [...](https://javascript.info/slots-composition#updating-slots), [..](https://javascript.info/slots-composition#summary) & slot-related methods [...](https://javascript.info/slots-composition#slot-api)
		- open/close menu example [...](https://javascript.info/slots-composition#menu-example)
- **styling**: `:host*`, `::slotted`, components custom CSS props [...](https://javascript.info/shadow-dom-style) & `pseudo` attr [..](https://javascript.info/shadow-dom#built-in-shadow-dom)
- **event handling**: event happen in shadow DOM **hidden to outside** --- retarget (bubble) to its **host** elem [..](https://javascript.info/shadow-dom-events)
	- evebt bubbling: `e.composedPath()`, `e.composed` (true for bubble across shadow DOM boundary) ...

# Template elem
`<template>` tag: for HTML markup storage (any syntax-valid HTML), content ignored by browser [..](https://javascript.info/template-element)
- elems not render, style/script not work, until `tmpl.content`: `DocumentFragment` [..](https://javascript.info/template-element#inserting-template) inserted to doc

# Generator
Regualr fn retur 1 value ==> Generators "yield" mtp values 1-by-1 (allow create data stream)
```javascript
function* generateSequence() {    // generator function
  yield 1;                        // no value af 'yield' means 'undefined'
  yield 2;
  return 3;
}

let generator = generateSequence(); // when called, return a Generator instead of run the fn 
alert(generator);                   // [object Generator]

// call generator.next() to get result
{value: 1, done: false}  
{value: 2, done: false} 
{value: 3, done: true}              // done:true for "return"
{done: true}                        // no more value af 'return'
```
- when called, it return a **Generator** (an Iterator with **next()** method) to manage the fn-exe
- `gen.next()` --- run fn-exe until `yield <value>` stmt & return `{value: <value>, done: true/false}`
  - [use generator to create iter-obj](https://javascript.info/generators#using-generators-for-iterables)
  - can use `gen` in `for..of` loop & sperad syntax to get yield values (exc returned value)
- pass data back to gen.yield
  - `gen.next(arg)` --- pass `arg` as the value of **current yield** & return the resulting-obj of **next yield** [..](https://javascript.info/generators#yield-is-a-two-way-street)
  - `gen.throw(err)` --- throw err as the result of **current yield** (err not catched in gen() will fall out to `gen.throw`)
- **Generator composition** --- embed gen-values into another gen-fn by `yield* gen(...)` 
- Task:  [pseudoRandom(seed)](https://javascript.info/generators#pseudo-random-generator)

[Async iteratable](https://javascript.info/async-iterators-generators#async-iterables) `async *[Symbol.asyncIterator]()`
- `async function*` for [async gen](https://javascript.info/async-iterators-generators#async-generators) --- yield value asyncly & `await gen.next()`
- `[Symbol.asyncIterator]()` for [async iterator](https://javascript.info/async-iterators-generators#async-iterators) --- iter over data that comes asyncly (e.g. paginated data) & use in `for await ..of` loop
- [the `async function* fetchCommits(repo)` example](https://javascript.info/async-iterators-generators#real-life-example)

# Programming pattern
**Fn decorator** [..](https://javascript.info/call-apply-decorators)
- fn-wrapper that takes fn as param (callback) and alters its behavior
- decorated fn lose prop --- `Proxy` ( forward eth to target) [..](https://javascript.info/proxy#proxy-apply)
- tasks: [spy(fn)](https://javascript.info/call-apply-decorators#spy-decorator), [delay(fn)](https://javascript.info/call-apply-decorators#delaying-decorator), [debounce(fn, ms)](https://javascript.info/call-apply-decorators#debounce-decorator), [throttle(fn, ms)](https://javascript.info/call-apply-decorators#throttle-decorator)

**Currying**
- transform `f(a, b, c)` → `f(a)(b)(c)` --- easily for partial (see below): let fn = f(a), then fn(b)(c) [..](https://javascript.info/currying-partials#currying-what-for)
- implementation [..](https://javascript.info/currying-partials#advanced-curry-implementation)

**Proxy & Reflect**
`Proxy`: a wrapper obj that intercepts ops (e.g. get/set props) on the target-obj by its handler (obj with traps) [..](https://javascript.info/proxy)
- ops on `proxy` => trigger crp trap => (if no) forwards ops to `target` [..](https://javascript.info/proxy#proxy) 
- proxy no own props, access from `target` [..](https://javascript.info/proxy#proxy)
- ops -> proxy handler methods -> internal methods (low level)
- Proxying a getter with `receiver` by `Reflect.get(target, prop, receiver);`: pass `receiver` (correct obj) as `this` in getter [...](https://javascript.info/proxy#proxying-a-getter)

`Reflect`: to complement Proxy [...](https://javascript.info/proxy#reflect)

Tasks: [array[-1]](https://javascript.info/proxy#accessing-array-1), [makeObservable(target)](https://javascript.info/proxy#observable)

**Recursion** [..](https://javascript.info/recursion)
- **base** (get result without nest calls) & **recursive step**
  - [execution context and stack]( "store info about the exe of currently running fn"), [recursion depth](https://javascript.info/recursion#two-ways-of-thinking "max # of nested fn calls, inc 1st one (i.e. max # of context in the stack), limited by JS engine"), [tail call optimization](https://javascript.info/recursion#sum-all-numbers-till-the-given-one "recur-call in last line (most JS engine not support)")
- **recursive vs iterative** [..](https://javascript.info/recursion#the-exit)
  - recursive: [take resrcs]( "nested calls and execution stack management [taks1]") but [simpler code]( "`shorter` & more understndable")
  - iterative: [memory-saving](https://javascript.info/recursion#the-exit "single context, changing var in the process, small fixed memory usage")
- recursive structure & linked list: `node = {value, next -> node2 / null}` 
- task: [Fib numbers](https://javascript.info/recursion#fibonacci-numbers), [Sum 1..n](https://javascript.info/recursion#sum-all-numbers-till-the-given-one), [Print list](https://javascript.info/recursion#output-a-single-linked-list), [Print list revresely](https://javascript.info/recursion#output-a-single-linked-list-in-the-reverse-order) 

# DOM
- range & selection [..](https://javascript.info/selection-range)
- mutation observer: fire callback when change observed on a DOM node, childList & subtree, attributes, text content [..](https://javascript.info/mutation-observer)
	- div `contentEditable` attr

# Module
- a script file that load each other by `export` & `import` [..](https://javascript.info/modules-intro#what-is-a-module), intro in ES6 [..](https://javascript.info/modules-intro)

**module in browser**: `<script type="module">` [..](https://javascript.info/modules-intro#what-is-a-module)
- `type` attr enable `import/export` [..](https://javascript.info/modules-intro#build-tools), [..](https://javascript.info/modules-intro#summary) --- the old "nomodule" attr [..](https://javascript.info/modules-intro#compatibility-nomodule)
- module `defer` [..](https://javascript.info/modules-intro#module-scripts-are-deferred) by default [..](https://javascript.info/modules-intro#summary), can `async` [..](https://javascript.info/modules-intro#async-works-on-inline-scripts) (work for both **inline**/external scripts [..](https://javascript.info/modules-intro#module-scripts-are-deferred), [..](https://javascript.info/modules-intro#async-works-on-inline-scripts)) 

`export/import` **syntax** [..](https://javascript.info/import-export#summary)
- default export (import without `{}`) vs named export [..](https://javascript.info/import-export#export-default)
- `default` keyword for default export [...](https://javascript.info/import-export#the-default-name) (when import, any name works --- naming consistency: crp to file name [..](https://javascript.info/import-export#a-word-against-default-exports))
- `as` for export/import renaming
- Re-export: `export .. from ..` (import & export): single entry point of fnality (mtp re-export stmt in 1 file) [..](https://javascript.info/import-export#re-export)
- `import "module"`: run module code without assign to var 

**dynamic imports** `import(<module-path>)`: return a **promise** resolves into a module-obj with exports (as keys) [..](https://javascript.info/modules-dynamic-imports)
- a special syntax, not a fn (like super(), cannot copy & use **call/apply**)
- work with rgl `<script>` tag (no need type="module")

**module vs rgl script** [..](https://javascript.info/modules-intro#core-module-features)
- auto strict (`this == undefined`)
- own scope (interchange fnality via `import/export` [..](https://javascript.info/modules-intro#summary))
- exe only once & export share when imported mtp times (use as init) 

**others**
- place of `import/export` stms not matter (script top or bottom), but not work inside "{...}" --- dynamic imports [..](https://javascript.info/import-export#summary)
- `import.meta`: obj contains info about the current module [..](https://javascript.info/modules-intro#import-meta)
- build tool (for deployment) e.g. **webpack** (bundle modules together & tree-shaking) [..](https://javascript.info/modules-intro#build-tools)
  - resulting “bundled” script `bundle.js` no `import/export` stmt ==> no `type=module` required (rgl script `src="bundle.js"`) [..](https://javascript.info/modules-intro#build-tools)

# Other concepts & Resources
- [code style & linter](https://javascript.info/coding-style)
- [Auto-testing lib & BDD](https://javascript.info/testing-mocha)
- [Manuals & specs](https://javascript.info/manuals-specifications)
- [Polyfill & Babel](https://javascript.info/polyfills) 
  - JS keep evolving & some features may not implemented in all engines
  - [Babel](https://babeljs.io/): transpiler (new syntax constructs) +  polyfill (updates/adds new fns)
  - [Langs “over” JS](https://javascript.info/intro#languages-over-javascript)
