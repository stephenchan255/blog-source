---
title: Async programming & network request in JavaScript
date: 2020-10-20 19:32:55
tags: [IT, Programming, JavaScript]
categories: [IT, Programming, JavaScript]
---

**async** actions: init now but finish later (code below it won't wait) [..](https://javascript.info/callbacks)

# Callback
`loadScript('xxx.js', (error, script) => { .. })` ("error-1st" cb style) [..](https://javascript.info/callbacks#handling-errors): cb hell / pyramid of doom
**Fn Call Scheduling**: `setTimeout/setInterval(..)` [..](https://javascript.info/settimeout-setinterval) (enqueue in macrotask [..](https://javascript.info/event-loop#event-loop))
```javascript
let timerId = setTimeout/setInterval(fn, delay=0, ...args); // keep fn-ref in scheduler (not GC) 
clearTimeout/clearInterval(timerId); // "timerId keep same" af clear (not null)
```
- **setTimeout()**: set `this=window` [..](https://javascript.info/call-apply-decorators#delaying-decorator)
- **setInterval()**: fn-exe-time > delay? [...](https://javascript.info/settimeout-setinterval#nested-settimeout)
- **nested setTimeout** vs setInterval ..
- tasks: [Output every second](https://javascript.info/settimeout-setinterval#output-every-second)
	
<!-- more -->

# Promise
`new Promise((resolve, reject) => .. ).then(value => .. ).catch(err => .. ).finally(fn)` [..](https://javascript.info/promise-basics)
- error occur in setTimeout (except `reject(err)`) not catched [..](https://javascript.info/promise-error-handling#error-in-settimeout)
- unhandlerd rejection & window `unhandledrejection` event [..](https://javascript.info/promise-error-handling#unhandled-rejections)
- cb vs promise: promise only single result (cb can be called mtp times) [..](https://javascript.info/promisify)

`async/await` [..](https://javascript.info/async-await#summary)
- `async` bf fn: return a promise & enable `await`
- `await` & `try-catch` --- promise.then() & catch()

Promise API
- `let promise = Promise.all/allSettled/race(...promises);` [..](https://javascript.info/promise-api)
	- `Promise.all()`: all resolved or reject
	- `Promise.allSettled()`: `[...{ status: 'fulfilled/rejected', value/reason: xxx }]`
	- `Promise.race()`: **1st settled** promise.result
- `Promise.resolve(value)/reject(err)`: a resolved / rejected promise, same as
`new Promise((resolve, reject) => resolve(value) || reject(error))`

# Abort async task: AbortController
- when call `ctrller.abort()` called [..](https://javascript.info/fetch-abort)
	- `ctrller.signal` emit `abort` event (`ctrller.signal.aborted` = true) 
	- `ctrller.signal.addEL('abort', handler)` 
- abort fetch op: `fetch(url, { signal: ctrller.signal })` [..](https://javascript.info/fetch-abort#using-with-fetch)
	- when `ctrller.abort()` called, fetch get event from `signal` then aborted & promise rejected with `AbortError`

# Network request
[AJAX](https://javascript.info/fetch "Async JS And XML"): load new info without page reload
- network method [..](https://javascript.info/formdata), [..](https://javascript.info/server-sent-events): fetcch
- built-in obj to make HTTP requests: XHR [..](https://javascript.info/xmlhttprequest)

## fetch & XHR
network method `fetch(url, [request-options])` [..](https://javascript.info/fetch) --- [fetch API](https://javascript.info/fetch-api)
- `fetch(url)`: [GET]( "download content of the url") request
- `request-options`: `{ method, headers: { 'Content-Type' .. }, body }` [..](https://javascript.info/fetch#post-requests), [..](https://javascript.info/fetch#request-headers)
	- body: [JSON](https://javascript.info/fetch#post-requests), [imgBlob](https://javascript.info/fetch#sending-an-image), [FormData](https://javascript.info/formdata) (inputs, file, Blob ..) [...](https://javascript.info/fetch#post-requests)

2-stage response
- `await fetch(..): response`: server response with headers (returned promise resolve with obj of Response) [..](https://javascript.info/fetch) 
	- `response.status`, `response.ok` (if HTTP-status 200-299 --- 404 / 500 do not cause promise reject)
	- `response.headers`: map-like obj [..](https://javascript.info/fetch#response-headers)
- get response body: `response.json()/blob() ..` (asyn) [...](https://javascript.info/fetch) 

progress track
- download: `response.body.getReader().read(): {done, value}` & `+response.headers.get('Content-Length')` [..](https://javascript.info/fetch-progress)
	- `response.body`: `ReadableStream`, repeatedly call read() to read body chunk-by-chunk & concat `value.length`
- upload: `xhr.upload.onprocess` & `e.loaded / e.total` [..](https://javascript.info/xmlhttprequest#upload-progress)
	- `xhr.upload.onprocess`: trigger when data sent [..](https://javascript.info/resume-upload#not-so-useful-progress-event) --- [resume file upload algorithm](https://javascript.info/resume-upload#algorithm)
	- `xhr.onprogress`: trigger when download response [..](https://javascript.info/xmlhttprequest#the-basics) (a data packet of the response arrived [..](https://javascript.info/xmlhttprequest#summary))

Task: [getGithubUsers(names)](https://javascript.info/fetch#fetch-users-from-github)

## CORS
- origin: "protocol://domain:port" [..](https://javascript.info/popup-windows#accessing-popup-from-window), [..](https://javascript.info/url#creating-a-url) & same origin policy: only allow to set `location` [..](https://javascript.info/cross-window-communication#same-origin) & get ref of `iframe.contentWindow` [..](https://javascript.info/cross-window-communication#in-action-iframe)
	- make same 2nd-level domain wins **same origin**: `document.domain = 'site.com'` [..](https://javascript.info/cross-window-communication#windows-on-subdomains-document-domain)
- cross-win msg (from any origin) --- win `postMessage(..)` interface [..](https://javascript.info/cross-window-communication#cross-window-messaging)
	- sender-win call `receiverWin.postMessage(..)` --- trigger `receiverWin.message` event in receiver-win
	- note: add handler to `receiverWin.message` event by `addEventHandler()` [..](https://javascript.info/cross-window-communication#onmessage)
- the old ways to work around cors: `<form>` & `<script>` [...](https://javascript.info/fetch-crossorigin#why-is-cors-needed-a-brief-history)
	- `<form target="iframe" ..>`: open action.url in ifame
	- script & JSONP protocol: `<script src="another-site.com?callback=xxx">`
		- remote dynamically generates a script, load and exe & call our local fn `xxx(data)`

## Fetch & CORS
Cross-origin requests & special response headers [...](https://javascript.info/fetch-crossorigin)
- [simple request](https://javascript.info/fetch-crossorigin#simple-requests): browser send header `Origin` --- `Access-Control-Allow-Origin` [...](https://javascript.info/fetch-crossorigin#cors-for-simple-requests)
- [non-simple request](https://javascript.info/fetch-crossorigin#simple-requests): browser extra "preflight" request (ask for permission 1st)
	- method `OPTIONS` + `A-C-Request-Method` / `*-Headers` --- `A-C-A-Origin`/`*-Methods` / `*-Headers` [...](https://javascript.info/fetch-crossorigin#non-simple-requests)
- js default accessible response headers --- `Access-Control-Expose-Headers` [...](https://javascript.info/fetch-crossorigin#response-headers)
- request with credentials [...](https://javascript.info/fetch-crossorigin#credentials), [..](https://javascript.info/fetch-api#credentials)
- request header: `Origin` vs `Referer` [...](https://javascript.info/fetch-crossorigin#why-do-we-need-origin) --- fetch `referrer, referrerPolicy` option [...](https://javascript.info/fetch-api)
- http code 301, 302 & fetch option `redirect` [...](https://javascript.info/fetch-api#redirect)
- window.onunload & fetch option `keepalive` [...](https://javascript.info/fetch-api#keepalive)  

## B/S communication: Polling & WebSocket
Get msg from server
- [Regular polling](https://javascript.info/long-polling#regular-polling): msg pass delay, server bombed with requests
- [Long polling](https://javascript.info/long-polling#long-polling): request (e.g. **fetch**) → server keep conn open until response with data → **response** process & re-request
	- good when msgs rare [..](https://javascript.info/long-polling#area-of-usage)
	- timeout status 502 
- [Server Sent Events](https://javascript.info/server-sent-events) `EventSource` --- vs `WebSocket`: ES support auto-reconn
	- `new EventSource(url, ..)`: start conn [..](https://javascript.info/server-sent-events#getting-messages) --- `open` (connected) / `error` event (response wrong `Content-Type` / HTTP status (e.g. 500 [..](https://javascript.info/server-sent-events#event-types)) [..](https://javascript.info/server-sent-events#reconnection))
	- server send data (response 200 & `Content-Type: text/..` to write text msg) [..](https://javascript.info/server-sent-events#getting-messages)
		- `message` event: `es.onmessage(e.data)`  
		- custom event: `es.addEL(..)` (data come with `event` field) [..](https://javascript.info/server-sent-events#event-types)
	- browser close conn: `es.close()` [..](https://javascript.info/server-sent-events#reconnection)
	- no reconn: response 204 | `error` event [..](https://javascript.info/server-sent-events#reconnection)
		- check if reconn when error: `es.onerror` `es.readyState === EventSource.CONNECTING` [..](https://javascript.info/server-sent-events#full-example)
	- response msg fields: `retry` (for timeout), `event` (for custom events), `data`, `id` (for reconn) ...

Data exchange: [WebSocket](https://javascript.info/websocket): persistent conn, no cors limitation [..](https://javascript.info/websocket#summary)
- `new WebSocket("wss://xxx	", ..)`: start conn [..](https://javascript.info/websocket#opening-a-websocket) --- `open` event: connected
	- `wss://` protocol (WebSocket over TLS) [..](https://javascript.info/websocket#a-simple-example)
- data exchange: `ws.send(data)` & `ws.onmessage(e.data)` [..](https://javascript.info/websocket#a-simple-example)
	- data type: only string/bin in browser ("frame") [..](https://javascript.info/websocket#data-transfer)
	- `ws.send(data)` onopen --- `ws.bufferedAmount`: # of bytes to be sent (when network slow) [..](https://javascript.info/websocket#rate-limiting)
- conn close: `ws.close(code=1000, reason);` & `ws.onclose`:`e.code/reason/wasClean` [...](https://javascript.info/websocket#connection-close)

# Storing data in the browser
## Cookie
Cookie: `name=value;` pairs stored in browser [..](https://javascript.info/cookie#reading-from-document-cookie)
- part of HTTP protocol [..](https://javascript.info/cookie)
	- request to server & server set a corresponding cookie using response `Set-Cookie` HTTP-header
	- subsequent request to the same domain, browser auto add them (in `Cookie` HTTP-header) , e.g. in auth
- read/write by `document.cookie` [..](https://javascript.info/cookie#reading-from-document-cookie), [..](https://javascript.info/cookie#writing-to-document-cookie)
	- 4kb/cookie, 20+/site (for network transfer limitation) [..](https://javascript.info/cookie#writing-to-document-cookie)
	- unaccessible to js if `httpOnly` option set [..](https://javascript.info/cookie#httponly)
- cookie options ...
	- by default, cookie rmed af browser close (session cookie) --- `expires` / `max-age` [..](https://javascript.info/cookie#expires-max-age)
	- `secure`: only accessible over HTTPS (if set in `https://`) [..](https://javascript.info/cookie#secure)
	- `samesite`: prevent XSRF attack --- not sent cookie if request comes from outside [..](https://javascript.info/cookie#samesite)
		- only work for browser af 2017, ignored by old browsers --- use with the old solution
		- **XSRF attack**: user submit a form to `bank.com` in `evil.com` & browser auto-sent auth-cookie
		- old solution: forms generated by `bank.com` have a special “XSRF protection token” & when receive form, `bank.com` checks for such token
- 3rd-party cookies: set by domain other than the page visiting [..](https://javascript.info/cookie#appendix-third-party-cookies)
	- the cookie bound to the domain, can use to track user when moves btn sites
  - GDPR enforce explicit user permission to set it

## Web storage obj: local/sessionStorage
save key/value pairs (str-only [..](https://javascript.info/localstorage#strings-only)) in browser by **web storage obj** [..](https://javascript.info/localstorage)
- `localStorage`: data shared btn all wins with the **same origin** & not expire [..](https://javascript.info/localstorage#localstorage-demo)
- `sessionStorage`: only **current browser tab** (even page refresh, but not re-open) [..](https://javascript.info/localstorage#sessionstorage)
- browser storage obj vs cookie: not sent to server, so larger storage (up to 5mb [..](https://javascript.info/localstorage#summary)) [..](https://javascript.info/localstorage)
- `window.onstorage` event: triggers when data updated in `local/sessionStorage` [...](https://javascript.info/localstorage#storage-event)
 - triggers on all win-obj where the storage accessible, except the causing one --- allow same origin wins to exchange msgs
- task: Autosave a form field

## indexedDB
indexedDB: mtp types of key/value, bigger storage (intended for offline apps), support transaction [...](https://javascript.info/indexeddb)
- `let openRequest = indexedDB.open(DBNname: str, schemaVersionNo: int);`: conn to a DB, bound to current origin
- openRequest obj events:
	- `error` & `console.error("Error", openRequest.error)`
	- `success` & `let db = openRequest.result`
	- `upgradeneeded`: when client local db version `e.oldVersion` < arg `schemaVersionNo` `e.newVersion`
		- e.g. user come 1st time (db not exist yet, version = 0), 
		- or client load old code (prevent by HTTP caching headers, or prompt reload page)
		- handler finishes without errs => trigger `openRequest.onsuccess`
- Parallel update problem: both old/new db conn exists (in diff tabs)
	- `versionchange` event trigger in tab1 (old version db conn): close old db conn & reload page (see `db.onversionchange`)
	- if old db conn not close,  `open()` in tab2 trigger `block` event (ask tab2 user to close other tabs for update)
	- variants ..
- delete db by `let deleteRequest = indexedDB.deleteDatabase(name)`
	- check `deleteRequest.onsuccess/onerror` for deletion result
- object store (i.e. table in RDB)
	- `db.create/deleteObjectStore(..), objectStoreNames.contains(..)` ..
	- created/removed/altered obj-store only in `upgradeneeded` handler (auto-generate a `versionchange` transaction)
	- can make data-ops outside `upgradeneeded` handler 
	- obj-store CRUD requests
		- search by key: `IDBKeyRange` obj & `store.get*([query])` (values sorted by key order) ..
			by other fields by `index` (return a obj-store-liked obj keyed by that field, could mtp-value) ..
		- add/update & delete data request: `store.add/put(obj)`, `store.delete(..)/clear()`
	- iter obj-store by `cursor` ..
- transaction
	- starts t & get it obj-store: `let t = db.transaction(..)` `let books = t.objectStore("books")`
	 & other request cmds
	- `request.onsuccess/error` & `request.result/error`
	- all t.requests finish => microtasks queue empty => auto-commit => `t.oncomplete` event
		- async ops/macrotask not allowed btn t.requests: t closed bf macrotasks starts => `TransactionInactiveError`
		- solution: split t ops & macrotasks/async ops apart ..
	- `t.abort()` & `t.onabort` event, `t.error`
		- failed request auto abort t, can be prevent in `request.onerror` handler by `e.preventDefault()`
- IndexedDB events bubble: `request` → `transaction` → `database`
	- `db.onerror = function(event) { let request = event.target.. }`
- Promise wrapper & `try..catch` vs Adding `onsuccess/onerror` to every request
- demo in summary ..