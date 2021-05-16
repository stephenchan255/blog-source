---
title: JavaScript & DOM
date: 2020-10-18 16:34:11
tags: [IT, Programming, JavaScript]
categories: [IT, Programming, JavaScript]
---

# JS intro
- init to **make webpages alive** [..](https://javascript.info/intro), run on platforms with **JS engine** (in browser aka "JS VM",  e.g. "V8" in Chrome & Opera) [..](https://javascript.info/intro#what-is-javascript)
	- browser env: `window` (root obj) => JS / DOM / BOM [..](https://javascript.info/browser-environment)
	- [BOM](https://javascript.info/browser-environment#bom-browser-object-model): `navigator` obj, `location.href`, model win fn: `alert()`, `prompt()`, `confirm()` [..](https://javascript.info/alert-prompt-confirm)
- JS in HTML: `<script>..</script>` & `<script src=..></script>` [..](https://javascript.info/hello-world) 
  - `type="text/javascript" language="javascript"` attr no need anymore [..](https://javascript.info/hello-world#modern-markup)
  - inline content ignored if `src` set [..](https://javascript.info/hello-world#external-scripts)
  - browser cache external script for re-use [..](https://javascript.info/hello-world#external-scripts)
- in-browser JS restrictions [...](https://javascript.info/intro#what-can-t-in-browser-javascript-do)

<!-- more -->

# DOM tree
- repre HTML/XML doc structure [..](https://javascript.info/dom-nodes#summary) (inc `<!DOCTYPE HTML>` [..](https://javascript.info/dom-nodes#other-node-types)), built when pages loading [..](https://javascript.info/dom-attributes-and-properties) (auto-correction [..](https://javascript.info/dom-nodes#autocorrection))
- rooted at `document` (repre the whole doc[..](https://javascript.info/dom-nodes#other-node-types)) --- `document.documentElement/.head /.body` [..](https://javascript.info/dom-navigation#on-top-documentelement-and-body)
- interact DOM elem in console [..](https://javascript.info/dom-nodes#interaction-with-console)

DOM node class hierarchy [..](https://javascript.info/basic-dom-node-properties#dom-node-classes)
- Node: Element, Text, Comment, Document (e.g. `HTMLDocument` instance `document` [..](https://javascript.info/basic-dom-node-properties#where-s-the-document-in-the-hierarchy))
- type check: `node.nodeName` / `elem.tagName` [..](https://javascript.info/basic-dom-node-properties#tag-nodename-and-tagname)

# HTML Attr & JS Prop
- **HTML-attr**: case-insensitive & value: str [..](https://javascript.info/dom-attributes-and-properties#html-attributes)
  - HTML `hidden` attr --- `style="display:none` [..](https://javascript.info/basic-dom-node-properties#the-hidden-property)
- **DOM prop**: case-sensitive & value: any type [..](https://javascript.info/dom-attributes-and-properties#dom-properties)
- access attr by elem methods [..](https://javascript.info/dom-attributes-and-properties#html-attributes)
- prop-attr sync [..](https://javascript.info/dom-attributes-and-properties#property-attribute-synchronization) & prop-attr type diff [..](https://javascript.info/dom-attributes-and-properties#dom-properties-are-typed)
- customized attr [..](https://javascript.info/dom-attributes-and-properties#non-standard-attributes-dataset): pass data to JS || “mark” HTML-elem for JS
  - naming: `data-*`, access: `dataset.xxx` (mtp-word camelCase)
  
# DOM modification
**Node Creation**: `document.createElement(tag: str)`[..](https://javascript.info/modifying-document#creating-an-element), `elem.cloneNode(..)` [..](https://javascript.info/modifying-document#cloning-nodes-clonenode)

**Node Selection** [..](https://javascript.info/searching-elements-dom)
- `elem.querySelector*(css: str)` --- css: `'tag[attr]'` [..](https://javascript.info/dom-attributes-and-properties#make-external-links-orange) & [..](https://javascript.info/searching-elements-dom#matches)
- other query methods [..](https://javascript.info/searching-elements-dom#summary) 
- **nav props** (child, parent, sibling, read-only) [..](https://javascript.info/dom-navigation)
- table props [..](https://javascript.info/dom-navigation#dom-navigation-tables)

## Node modification
### Content
- `elem.innerHTML` [..](https://javascript.info/basic-dom-node-properties#innerhtml-the-contents)
  - innerHTML += xxx: replace instead addition (resrc reload, selection & input rmed)
  - insert `<script>` by innerHTML: inserted but not executed
  - elem.**outerHTML** = innerHTML + elem itself [..](https://javascript.info/basic-dom-node-properties#outerhtml-full-html-of-the-element)
    - write to elem.outerHTML: replace node in DOM but **not change elem** itself (var still ref to the old value) --- query again
- `elem.textContent`: inner text without tags, insert as text (safer then `innerHTML`)
- `Text/Comment.data`: text/cmt content of the node (similar to **nodeValue** prop) [..](https://javascript.info/basic-dom-node-properties#nodevalue-data-text-node-content)
- task: [Count descendants](https://javascript.info/basic-dom-node-properties#count-descendants), [Create a calendar](https://javascript.info/modifying-document#create-a-calendar), [Sort the table](https://javascript.info/modifying-document#sort-the-table)

### Style
css class style (prefer [..](https://javascript.info/styles-and-classes))
- read class style: `getComputedStyle(elem, ..).<camelCaseProp>`: str with unit (of resolved value), e.g. "20px" [..](https://javascript.info/styles-and-classes#computed-styles-getcomputedstyle)
- set class: `elem.className` (whole replace) / `elem.classList` (single class) [..](https://javascript.info/styles-and-classes#classname-and-classlist) 

style-attr style: `elem.style.<camelCaseProp>`: str with unit, e.g. "100px", writable [..](https://javascript.info/styles-and-classes#element-style)
  - rm style : `elem.style.xxx = ''`
  - full rewrite `elem.style.cssText = xxx` || `setAttribute('style', cssText)` [..](https://javascript.info/styles-and-classes#resetting-the-style-property)

### Size, coords & scroll
geometry props: return num (in "px") [..](https://javascript.info/size-and-scroll#geometry)
- Size props: `offset*`, `client*` [...](https://javascript.info/size-and-scroll#geometry)
- scrolled props: `scroll*` [...](https://javascript.info/size-and-scroll#scrollwidth-height), [...](https://javascript.info/size-and-scroll#scrollleft-scrolltop)
	```javascript
  elem.style.height = `${elem.scrollHeight}px`; // expand elem to full height
  
  elem.scrollTop = elem.scrollHeight; // auto-scroll up [Events mouseover/mouseout, relatedTarget]
  elem.scrollTop = 0 / Infinity       // scroll to top/bottom
  ```
	- get doc scrolled-out part size: `window.pageX/YOffset` (num in px) [..](https://javascript.info/size-and-scroll-window#page-scroll)
	- scroll doc: `window.scrollBy/To(..)` [..](https://javascript.info/size-and-scroll-window#window-scroll) & `elem.scrollToView(top=true)`: scroll page to make elem top / bottom of the win
	- enable/forbid scroll: `style.overflow[X/Y]='scroll/hidden'` (same as CSS overflow[-x/-y]) [..](https://javascript.info/size-and-scroll#what-is-the-scrollbar-width) & [..](https://javascript.info/size-and-scroll-window#forbid-the-scrolling)
- coords props [..](https://javascript.info/coordinates): 
	- `clientX/Y` (rel to win, css position: **fixed**) & `pageX/Y` (rel to doc, css position: **absolute**) [..](https://javascript.info/coordinates), [..](https://javascript.info/coordinates#getCoords)
		- `document.elementFromPoint(clientX, clientY)`: most nested elem, null if out of win [..](https://javascript.info/coordinates#elementFromPoint)
	- get coords: 
		- `elem.getBoundingClientRect()` return num (in "px") [...](https://javascript.info/coordinates#element-coordinates-getboundingclientrect)
		- `style.left/top` [..](https://javascript.info/size-and-scroll#place-the-ball-in-the-field-center), `style.width/height` [..](https://javascript.info/size-and-scroll#what-is-the-scrollbar-width) => **str with unit**
	- tasks: [Find win coords of the field](https://javascript.info/coordinates#find-window-coordinates-of-the-field), [Position the note inside (absolute)](https://javascript.info/coordinates#position-the-note-inside-absolute)

Notes
- text overflow --- padding-bottom area may filled with text [..](https://javascript.info/size-and-scroll#sample-element)
- for elem width/height/distance, **use geometry prop (e.g. clientWidth) instead** getComputedStyle() [..](https://javascript.info/size-and-scroll#don-t-take-width-height-from-css), [..](https://javascript.info/size-and-scroll#the-difference-css-width-versus-clientwidth)
- get win size: `document.documentElement.clientWidth/H` (without sbar)  vs `window.innerWeight/H` (inc sbar) [..](https://javascript.info/size-and-scroll-window#width-height-of-the-window)
- get doc size (with scroll out part) [..](https://javascript.info/size-and-scroll-window#width-height-of-the-document)

## Node insert, move & rm 
- insert (replace) as text: `node.append/prepend/before/after/replaceWith(..)` [..](https://javascript.info/modifying-document#insertion-methods)
	- move nodes by inserting existing one (auto rm from old place) [..](https://javascript.info/modifying-document#node-removal)
- insert as HTML: `elem.insertAdjacentHTML(..)` [..](https://javascript.info/modifying-document#insertadjacenthtml-text-element) 
- removal: `node.remove()` [..](https://javascript.info/modifying-document#node-removal)
- the old insert/rm methods [..](https://javascript.info/modifying-document#old-school-insert-remove-methods) & `document.write(html: str)` [..](https://javascript.info/modifying-document#a-word-about-document-write): call when onload to write html into page “right here and now” (call af load replace page content)

# Event handling
## Event & event obj
events list [...](https://javascript.info/introduction-browser-events)

event happen -> browser create event-obj (puts details into it) & pass as arg to event handler [..](https://javascript.info/introduction-browser-events#event-object) 

Event obj: `e.type` (e.g. "click") [..](https://javascript.info/introduction-browser-events#event-object)

Event Bubbling & event delegation pattern
- 3 Event phase ([Capturing](https://javascript.info/bubbling-and-capturing#capturing), Target, [Bubbling](https://javascript.info/bubbling-and-capturing#bubbling) --- `e.eventPhase` [..](https://javascript.info/bubbling-and-capturing#summary))
- event delegation (handle event on container for mtp elems) [..](https://javascript.info/event-delegation)
- `e.target`, `e.currentTarget` [..](https://javascript.info/bubbling-and-capturing#event-target)
- stop bubbling [...](https://javascript.info/bubbling-and-capturing#stopping-bubbling)
- tasks: [Hide messages with delegation](https://javascript.info/event-delegation#hide-messages-with-delegation), [toogle tree menu](https://javascript.info/event-delegation#tree-menu), [Sortable table](https://javascript.info/event-delegation#sortable-table), [Tooltip behavior](https://javascript.info/event-delegation#tooltip-behavior)

script-gen event [..](https://javascript.info/dispatch-events) 
- construct [..](https://javascript.info/dispatch-events#event-constructor) & [..](https://javascript.info/dispatch-events#mouseevent-keyboardevent-and-others), dispatch [..](https://javascript.info/dispatch-events#dispatchevent)
- diff user event & script-gen event: `e.isTrusted` [..](https://javascript.info/dispatch-events#dispatchevent)
- custom event & `detail` prop [..](https://javascript.info/dispatch-events#custom-events), [..](https://javascript.info/dispatch-events#summary)

Event Processing Queue: Asycnly & Syncly [..](https://javascript.info/dispatch-events#events-in-events-are-synchronous) 

## Event handler: fn(e)
add/rm Event handler
- `elem.addEventListener(e-type, handler, ..)` [..](https://javascript.info/introduction-browser-events#addeventlistener)
	- work for custom event [..](https://javascript.info/dispatch-events#bubbling-example); allow mtp handlers (call it mtp times)
	- support `obj.handleEvent(e)` as handler [..](https://javascript.info/introduction-browser-events#object-handlers-handleevent)
- HTML attr `on<event>="fn-body"` [..](https://javascript.info/introduction-browser-events#possible-mistakes) || DOM prop `elem.on<event> = fn` [..](https://javascript.info/introduction-browser-events#event-handlers), e.g. onclick
  - built-in events [..](https://javascript.info/dispatch-events#bubbling-example), 1 handler [..](https://javascript.info/introduction-browser-events#dom-property)

Others
- `this` in handler: e.currentTarget [..](https://javascript.info/introduction-browser-events#accessing-the-element-this) & [..](https://javascript.info/introduction-browser-events#event-object)
- `elem.setAttribute('on<event>', handler)` not work for event handler [..](https://javascript.info/introduction-browser-events#possible-mistakes)
- Tasks: [Move the ball across the field](https://javascript.info/introduction-browser-events#move-the-ball-across-the-field), [Create a sliding menu](https://javascript.info/introduction-browser-events#create-a-sliding-menu), [Add a closing button](https://javascript.info/introduction-browser-events#add-a-closing-button), [Carousel](https://javascript.info/introduction-browser-events#carousel)

## Prevent default actions
`e.preventDefault()` on handler || ` on<event>` handler return false  [..](https://javascript.info/default-browser-action#preventing-browser-actions)
- `e.defaultPrevented` (status check): true (means the event is handled somewhere) [..](https://javascript.info/default-browser-action#event-defaultprevented) & `elem.dispatchEvent(event)` return false [..](https://javascript.info/dispatch-events#event-preventdefault)
- `passive: true` option of `addEventListener`: signals that the handler not to call preventDefault() [why needed?](https://javascript.info/default-browser-action#the-passive-handler-option)
- default browser action list [..](https://javascript.info/default-browser-action#summary)
- Tasks: [Why "return false" doesn't work?](https://javascript.info/default-browser-action#why-return-false-doesn-t-work), [Image gallery](https://javascript.info/default-browser-action#image-gallery)

# Mouse Event
- events list [...](https://javascript.info/mouse-events-basics#mouse-event-types)
- mouse coords: `e.clientX/Y, e.pageX/Y` [..](https://javascript.info/mouse-events-basics#coordinates-clientx-y-pagex-y), mouse btn: `e.button` (=0 left-btn, =1 middle, =2 right-btn) [..](https://javascript.info/mouse-events-basics#mouse-button)
- mouse event also inc info of modifier keys: `e.shiftKey/altKey/ctrlKey/metaKey`(Mac): bool (true if pressed) [..](https://javascript.info/mouse-events-basics#modifiers-shift-alt-ctrl-and-meta)
- disable selection (on dblclick) & copy: `onmousedown/oncopy return false` [..](https://javascript.info/mouse-events-basics#preventing-selection-on-mousedown)

## Mouse & Pointer Event
`mousemove` [..](https://javascript.info/mouse-events-basics#coordinates-clientx-y-pagex-y), `mouseover/out` event & `e.relatedTarget` (diff with `mouseenter/leave`) [..](https://javascript.info/mousemove-mouseover-mouseout-mouseenter-mouseleave#summary)
- event delegation example [..](https://javascript.info/mousemove-mouseover-mouseout-mouseenter-mouseleave#event-delegation)
- task: ["Smart" tooltip](https://javascript.info/mousemove-mouseover-mouseout-mouseenter-mouseleave#smart-tooltip) (tooltip style)
- impl by mouse events: overall algorithm [..](https://javascript.info/mouse-drag-and-drop#drag-n-drop-algorithm), pointer correct positioning [..](https://javascript.info/mouse-drag-and-drop#correct-positioning), drappable elem hightlight [..](https://javascript.info/mouse-drag-and-drop#potential-drop-targets-droppables)
  - draggable elem always above other elem when dragging & mouse event only happen on the top elem, so eh on other elem not work, can fix by
  ```javascript
    draggble.hidden = true;
    let elemBelow = document.elementFromPoint(e.clientX, event.clientY);
    draggble.hidden = false;
  ```
	- task: [Slider](https://javascript.info/mouse-drag-and-drop#slider), [Drag superheroes around the field](https://javascript.info/mouse-drag-and-drop#drag-superheroes-around-the-field) (not checked yet)
- Pointer Event: handling mouse, touch and pen events simultaneously, with a single piece of code [..](https://javascript.info/pointer-events#summary)

## Scroll Event
- `scroll` event on elem & win [..](https://javascript.info/onscroll)
- task: [Endless page](https://javascript.info/onscroll#endless-page), [Up/down button](https://javascript.info/onscroll#up-down-button), [Load visible images](https://javascript.info/onscroll#load-visible-images)

# Keyboard Event
`keydown`, `keyup` [..](https://javascript.info/keyboard-events#keydown-and-keyup)
-  `e.code` (physical key loc, e.g "KeyZ", "ShiftLeft") vs `event.key` (meaning of key, ie. char inputted, e.g. "z" || "Z") [..](https://javascript.info/keyboard-events#event-code-and-event-key)
- key repeat: key pressed for a long enough time [..](https://javascript.info/keyboard-events#auto-repeat)
- task: [Extended hotkeys](https://javascript.info/keyboard-events#extended-hotkeys)

# Form Ctrl
- named collection: `document.forms` & `form/fieldset.elements` (fieldset as subform) [..](https://javascript.info/form-elements#navigation-form-and-elements)
- back-ref: `elem.form` [..](https://javascript.info/form-elements#backreference-element-form)
- Form elements: input/textarea, select and option [..](https://javascript.info/form-elements#form-elements)

## Focusing Event
`focus` / `blur` event [..](https://javascript.info/focus-blur#events-focus-blur) & `elem.focus/blur()` [..](https://javascript.info/focus-blur#methods-focus-blur)
- event not bubble (but capturable) --- bubble version `focusin` / `focusout` event (handler assign by `elem.addEventListener`)

Others
- get current focused elem: `document.activeElement` [..](https://javascript.info/focus-blur#summary) --- css `:focus` [..](https://javascript.info/focus-blur#allow-focusing-on-any-element-tabindex)
- `autofocus` attr [..](https://javascript.info/focus-blur)
- enable any elem focusable: `tabindex` attr || `elem.tabindex` [..](https://javascript.info/focus-blur#allow-focusing-on-any-element-tabindex)
- input validate attr: `required`, `pattern` [...](https://javascript.info/focus-blur#events-focus-blur)

task: [Editable div](https://javascript.info/focus-blur#editable-div), [Edit TD on click](https://javascript.info/focus-blur#edit-td-on-click), [Keyboard-driven mouse](https://javascript.info/focus-blur#keyboard-driven-mouse)

## Data update events
- `change` event: on finish change, e.g. text-input elem blur, select-opt changes [..](https://javascript.info/events-change-input#event-change)
- `input` event: on any value change [..](https://javascript.info/events-change-input#event-input)
- `cut/copy/paste` event [...](https://javascript.info/events-change-input#events-cut-copy-paste)

## Form submit event
`submit` event on form (sent data to server): 
- trigger on press "Enter" on input field => trigger **click** on input[type=submit] [..](https://javascript.info/forms-submit#event-submit)
- submit form manually: `form.submit()`:  (`submit` event not generated)
- tasks: [Modal form](https://javascript.info/forms-submit#modal-form)

# Doc & resrc loading
## Page load events
- `document.DOMContentLoaded`: HTML & DOM tree ready, form autofill [..](https://javascript.info/onload-ondomcontentloaded#built-in-browser-autofill)
	- see `<img>` node but size = 0
	- **wait non-async script** exe [..](https://javascript.info/onload-ondomcontentloaded#domcontentloaded-and-scripts), but **not wait external-CSS** (unless script require it) [..](https://javascript.info/onload-ondomcontentloaded#domcontentloaded-and-styles)
	- handle by `addEventListener()` [..](https://javascript.info/onload-ondomcontentloaded#domcontentloaded)
- `window.load`: external resrc (css, img) loaded (img size known) [..](https://javascript.info/onload-ondomcontentloaded#window-onload)
- `window.beforeunload/unload`: user leaving this page: nav to other pages/close win [..](https://javascript.info/onload-ondomcontentloaded#window.onbeforeunload)
	- `window.onbeforeunload` return false / non-empty str: cancel the event (prevent default?) & show the str as msg e.g. "change not save, sure to leave?" --- answer negative block page-nav? [..](https://javascript.info/clickjacking#blocking-top-navigation)

Others
- `document.readState`: "loading", "interactive" , "complete" --- the old **document.readystatechange** [..](https://javascript.info/onload-ondomcontentloaded#readystate)
- Whole process [...](https://javascript.info/onload-ondomcontentloaded#readystate)

## External script attr: async, defer
Both only for external script (ignored if no `src` attr) [..](https://javascript.info/script-async-defer#defer), [..](https://javascript.info/modules-intro#async-works-on-inline-scripts) and load not-block (load script in bg)
- `async`: run in load-1st order [..](https://javascript.info/script-async-defer#async)
	- Dynamic scripts (gen by JS) load af insert to the doc [..](https://javascript.info/script-async-defer#dynamic-scripts)	
- `defer`: run (in doc order) when DOM ready (but bf DOMContentLoaded event)  [..](https://javascript.info/script-async-defer#defer)

## Resrc loading event
Most resrcs start loading when added to the doc, except `<img>` (load when gets src & cached [..](https://javascript.info/onload-onerror#load-images-with-a-callback))
- `load/error` events: external resrc (tag with `src` attr [..](https://javascript.info/onload-onerror#other-resources), e.g. script, img, css) loaded successfully / err  [..](https://javascript.info/onload-onerror)
- iframe.onload: on loading finished ( successful || error) (history) [..](https://javascript.info/onload-onerror#other-resources)
- track other errors: `window.onerror` [..](https://javascript.info/onload-onerror#script-onerror)
- CORS & script `crossorigin` attr [...](https://javascript.info/onload-onerror#crossorigin-policy)