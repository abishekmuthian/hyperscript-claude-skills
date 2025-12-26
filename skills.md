---
name: hyperscript
description: Write and debug hyperscript code for front-end web development. Use when working with hyperscript files, HTML with _ attributes, DOM manipulation, event handlers, behaviors, web workers, or HTMX integration. Covers all syntax, commands, expressions, and patterns.
---

# Hyperscript Skills Documentation

## Overview

Hyperscript is a front-end scripting language designed for DOM manipulation and event handling with an emphasis on "Locality of Behavior" (LoB). It embeds code directly on HTML elements using the `_` attribute and uses English-like syntax derived from the xTalk family (HyperTalk heritage).

### Installation

```html
<!-- Via CDN -->
<script src="https://unpkg.com/hyperscript.org@0.9.14"></script>
```

Or import in build steps and call `_hyperscript.browserInit()`.

## Core Concepts

### Basic Structure

Hyperscript consists of three primary components:
1. **Features** - primarily event handlers
2. **Commands** - statements that perform actions
3. **Expressions** - values and operations

### Comments

```hyperscript
-- Single line comment (hyperscript style)
// Single line comment (JavaScript style)
/* Multi-line
   comment */
```

### Command Separators

- Commands separated by `then` or line breaks
- Blocks terminated with `end`

```hyperscript
on click
  add .active to me
  then wait 1s
  then remove .active from me
end
```

## Variables & Scoping

### Scope Types

Hyperscript supports three scope levels:

1. **Local** - Function/handler scoped
2. **Element** - Element-specific, shared across features
3. **Global** - Application-wide

### Naming Conventions

```hyperscript
-- Global scope ($ prefix)
set $globalVar to 10

-- Element scope (: prefix)
set :elementVar to 20

-- Explicit scope declarations
set global myGlobal to 30
set element myElement to 40
set local myLocal to 50
```

### Attributes

Store data as DOM attributes using `@` syntax:

```hyperscript
-- Set attribute
set @data-count to 10

-- Read attribute
set x to @data-count

-- Attribute literal with filter
set links to [@href='/example']
```

## Special Symbols & Variables

Implicit variables available contextually:

```hyperscript
me, my, I          -- Current element
it, result         -- Last command result
event              -- Triggering event
target             -- Original event target
body               -- Document body
the                -- Whitespace for readability
```

### Examples

```hyperscript
on click
  add .clicked to me
  log my innerHTML
  send customEvent to target
end
```

## Literals

### Common Literals

```hyperscript
-- Numbers
42
3.14
-10

-- Booleans
true
false

-- Strings
"double quotes"
'single quotes'
`template ${expression}`

-- Null
null

-- Collections
[1, 2, 3, 4]
{foo: "bar", baz: 42}
```

### DOM Literals

```hyperscript
-- ID selector
#myElement

-- Class selector
.myClass

-- Query selector literal
<div.header/>
<button[type='submit']/>

-- Attribute reference
@href
@data-value

-- Style property
*opacity
*background-color

-- Measurements
1em
35px
2rem
10ms
2s
```

### Time Literals

```hyperscript
2s              -- 2 seconds
10ms            -- 10 milliseconds
10 seconds      -- verbose form
500 milliseconds
```

## Expressions

### Operators

#### Comparison Operators

```hyperscript
-- Standard
x > 5
x < 10
x == 10
x != 0

-- Natural language
x is 10
x is not 0
x am valid
x am not ready

-- Specialized
element matches <.active/>  -- CSS selector matching
container contains element  -- Element containment
value exists                -- Not null/undefined
array is empty              -- Empty check
x is no value              -- Null check
```

#### Arithmetic Operators

```hyperscript
x + 5
x - 3
x * 2
x / 4
x mod 3

-- Must be fully parenthesized when combined
(x + 5) * 2
```

### Property Access

```hyperscript
-- Dot notation
object.property
element.innerHTML

-- Bracket notation
object['property']
array[0]

-- Possessive form
object's property
my innerHTML
its value
window's location

-- Of expression
property of object
innerHTML of element
location of the window
```

### Null-Safe Access

All property access in hyperscript is null-safe - returns `null` if object is `null`:

```hyperscript
-- Will not throw error if element is null
set value to element.innerHTML
```

### Conversions

```hyperscript
10 as String       -- "10"
"10" as Int        -- 10
"10.5" as Float    -- 10.5
object as JSON     -- JSON string
jsonString as Object  -- Parse JSON
```

### Positional Expressions

```hyperscript
-- First/Last/Random
the first <div/>
the last <section/>
the random <li/>

-- Relative positioning
the next <div/>
the previous <div/>

-- Closest (ancestor search)
the closest <section/>
the closest parent <div/>
```

### In Expression (Scoped Queries)

```hyperscript
-- Query within specific context
(<p/> in me).innerHTML
the first <li/> in #myList
```

### Array Operations

Properties on arrays automatically flat-map:

```hyperscript
-- Gets innerHTML from all <p> elements
set contents to <p/>.innerHTML
```

### Closures/Lambda Functions

Haskell-style syntax for inline functions:

```hyperscript
set strs to ["a", "list", "of", "strings"]
set lens to strs.map(\ s -> s.length)

-- Multi-parameter
set results to items.reduce(\ acc, item -> acc + item.value, 0)
```

### Cookies

Hyperscript provides localStorage-like API for cookies:

```hyperscript
set cookies.myCookie to "value"
log cookies.myCookie
```

## Commands

### Variable Assignment

#### set

```hyperscript
set x to 10
set y to x + 5
set element.innerHTML to "Hello"
set @data-count to 0
```

#### put

Places content into/before/after elements:

```hyperscript
put "Clicked!" into me
put newElement before #existing
put content after .target
put value at the start of #list
put value at the end of #list
```

### DOM Manipulation

#### add

```hyperscript
-- Add class
add .active to me
add .disabled to #myButton

-- Add attribute
add @disabled to <button/>
```

#### remove

```hyperscript
-- Remove class
remove .active from me
remove .hidden from <div/>

-- Remove attribute
remove @disabled from <button/>

-- Remove element from DOM
remove me
remove .elements-to-remove
remove the first <li/> in #myList
```

#### toggle

```hyperscript
-- Toggle class
toggle .active on me
toggle .visible on #panel

-- Toggle attribute
toggle @disabled on <button/>
```

#### show/hide

```hyperscript
-- Using display (default)
show me
hide #modal

-- Using visibility
show element with *visibility
hide element with *visibility

-- Using opacity
show element with *opacity
hide element with *opacity
```

### Visual Effects

#### transition

Animates style property changes:

```hyperscript
on click
  transition my *opacity to 0
  then remove me
end

-- Multiple properties
transition element's *width to 200px, *height to 300px over 2s
```

#### settle

Waits for CSS transitions/animations to finish:

```hyperscript
on click
  add .fade-out to me
  settle
  remove me
end
```

### Asynchronous Operations

#### wait

```hyperscript
-- Wait for duration
wait 2s
wait 500ms
wait 2 seconds

-- Wait for event
wait for click
wait for myCustomEvent
wait for a continue or 3s  -- Wait for event OR timeout
```

#### fetch

HTTP requests with transparent promise handling:

```hyperscript
-- Basic GET
fetch /api/data
put it into #result

-- POST with body
fetch /api/save with method:'POST', body:myData
if it.ok
  log "Saved!"
end

-- Naked string URLs
fetch /data
fetch https://api.example.com/users
```

### Event Handling

#### send/trigger

Dispatch custom events:

```hyperscript
-- Send to element
send myEvent to #target

-- Send with data
send dataUpdate(x: 10, y: 20) to #chart

-- Trigger on element
trigger click on <button/>
```

#### halt

Stop event propagation and prevent default:

```hyperscript
on click
  halt the event
  -- Handler continues but event stops
end
```

### Control Flow

#### if/else

```hyperscript
if x > 10
  log "Greater than 10"
else if x > 5
  log "Greater than 5"
else
  log "5 or less"
end

-- Unless modifier (inline conditional)
log "Valid" unless x is 0
add .active to me if x > 5
```

#### repeat/for loops

```hyperscript
-- Iterate array
repeat for item in [1, 2, 3, 4]
  log item
end

-- Iterate with index
repeat for item, index in myArray
  log index, item
end

-- While loop
repeat while x < 10
  increment x
end

-- Until loop
repeat until x == 10
  increment x
end

-- Times loop
repeat 5 times
  log "Hello"
end

-- Forever (use with break)
repeat forever
  if condition then break end
  wait 1s
end

-- Loop control
repeat for item in items
  if item is 0 then continue end
  if item > 100 then break end
  log item
end
```

### Functions

#### def

Define functions:

```hyperscript
-- Basic function
def greet(name)
  return "Hello, " + name
end

-- No return value
def logMessage(msg)
  log msg
  exit  -- Optional explicit exit
end

-- Namespaced function
def utils.increment(i)
  return i + 1
end

-- With exception handling
def riskyOperation()
  call mightFail()
catch e
  log "Error:", e
  return null
finally
  log "Cleanup"
end
```

#### call/get

Invoke functions:

```hyperscript
call myFunction()
call utils.increment(5)

-- Get result
get utils.increment(5)
log it  -- result available as 'it'

-- Inline
log utils.increment(5)
```

### Output & Debugging

#### log

```hyperscript
log "Message"
log variable
log "Value:", x, "Count:", count
```

#### beep (debugging operator)

Pass-through debugging that logs without disrupting code flow:

```hyperscript
set x to beep! someExpression
-- Logs source, value, and type, then returns value
```

### Object Creation

#### make

Create object instances:

```hyperscript
make a URL from "/path/" called myUrl
make a FormData from <form/> called formData
make a Date from "2024-01-01" called myDate
```

### Math Operations

#### increment/decrement

```hyperscript
increment x
decrement y
increment x by 5
decrement @data-count by 2

-- Works with string-to-number conversion
increment element.textContent
```

### Array/String Operations

#### append

```hyperscript
-- Append to array
append item to myArray

-- Append to string
append "!" to message

-- Append to DOM
append <div>New</div> to #container
```

### Element Measurement

#### measure

Get element dimensions and positions:

```hyperscript
measure my top
measure my left
measure my width
measure my height
measure element's offsetWidth
```

### Error Handling

#### throw

```hyperscript
if not valid
  throw "Invalid input"
end

throw {message: "Error", code: 404}
```

## Features (Event Handlers)

### Basic Event Syntax

```hyperscript
on click
  add .clicked to me
end

on mouseover
  add .hover to me
end

on mouseout
  remove .hover from me
end
```

### Event Filters

Use brackets to filter events:

```hyperscript
-- Only left clicks
on click[button == 0]
  log "Left click"
end

-- Only with modifier keys
on click[event.altKey]
  log "Alt + Click"
end

on click[event.ctrlKey]
  log "Ctrl + Click"
end

-- Key filtering
on keydown[key == 'Escape']
  hide #modal
end

on keydown[key == 'Enter']
  call submitForm()
end
```

### Event Destructuring

Extract event properties into local variables:

```hyperscript
on mousedown(button)
  put the button into the next <output/>
end

on keydown(key)
  log "Key pressed:", key
end
```

### Event Queueing Strategies

Control how events queue when handlers are executing:

```hyperscript
-- Default: queue last (most recent event takes priority)
on click
  wait 2s
  log "Done"
end

-- Queue none: discard incoming events
on click queue none
  wait 2s
  log "Only first click processed"
end

-- Queue all: preserve every event
on click queue all
  wait 1s
  log "Every click will be processed"
end

-- Queue first: queue only the first event
on click queue first
  wait 2s
  log "First queued event processed"
end

-- Every: execute immediately for each event
on every click
  log "Executes immediately"
end
```

### Special Events

#### mutation

Monitor DOM changes via MutationObserver:

```hyperscript
on mutation of @class
  log "Class attribute changed"
end

on mutation of childList
  log "Children changed"
end
```

#### intersection

Track element visibility via IntersectionObserver:

```hyperscript
on intersection
  add .visible to me
end

on intersection(intersecting)
  if intersecting
    add .visible to me
  else
    remove .visible from me
  end
end
```

#### HTMX Integration

```hyperscript
on htmx:beforeRequest
  add .loading to me
end

on htmx:afterOnLoad
  remove .loading from me
end

-- Disable button during request
on htmx:beforeRequest
  toggle @disabled until htmx:afterOnLoad
end
```

### Event Handler Examples

```hyperscript
-- Click counter
<button _="
  on click
    increment @data-count
    put @data-count into me
  end
">0</button>

-- Fade and remove
<div _="
  on click
    transition my *opacity to 0
    settle
    remove me
  end
">Click to remove</div>

-- Toggle visibility
<button _="on click toggle .hidden on #panel">Toggle Panel</button>

-- Fetch and display
<button _="
  on click
    fetch /api/data
    put it into #result
  end
">Load Data</button>

-- Debounced search
<input _="
  on keyup
    wait 500ms
    fetch `/search?q=${my value}`
    put result into #results
  end
">
```

## Behaviors

Behaviors bundle reusable hyperscript code:

```hyperscript
-- Define behavior
behavior Removable
  on click
    transition my *opacity to 0
    settle
    remove me
  end
end

-- Install behavior
<div _="install Removable">Click to remove</div>

-- Behavior with parameters
behavior Draggable(handle)
  on pointerdown from handle
    -- dragging logic
  end
end

-- Install with parameters
<div _="install Draggable(handle: #handle)">
  <div id="handle">Drag handle</div>
</div>
```

## Web Workers

Execute hyperscript in isolated sandboxed environment:

```hyperscript
worker Computation
  def fibonacci(n)
    if n <= 1
      return n
    else
      return fibonacci(n - 1) + fibonacci(n - 2)
    end
  end
end

-- Call worker function
on click
  call Computation.fibonacci(40)
  put it into #result
end
```

## Web Sockets

Bidirectional server communication:

```hyperscript
socket ChatSocket ws://server.com/chat
  on open
    log "Connected"
    send message: {type: 'join', user: 'Alice'} to ChatSocket
  end

  on message as json
    put message.text into #chatLog
  end

  on close
    log "Disconnected"
  end
end

-- Send messages
<button _="
  on click
    send message: {text: #input.value} to ChatSocket
  end
">Send</button>
```

## Event Source (Server-Sent Events)

Uni-directional server-to-client communication:

```hyperscript
eventsource Updates /api/updates
  on message as json
    put message into #notifications
  end

  on open
    log "SSE connected"
  end

  on error
    log "SSE error"
  end
end
```

## Inline JavaScript

Use `js` keyword for performance-critical code:

```hyperscript
def computeHeavy(data)
  js(data)
    // JavaScript code here
    let result = data.map(x => x * x).reduce((a, b) => a + b);
    return result;
  end
end

-- Inline JS expression
set result to js(x, y) x * y + Math.sqrt(x) end
```

## Advanced Patterns

### Async Transparency

Hyperscript automatically handles Promises without callbacks:

```hyperscript
on click
  fetch /api/step1
  put it into $step1Data

  fetch /api/step2
  put it into $step2Data

  fetch /api/step3
  put it into #result
end
```

### Forcing Async Execution

Use `async` keyword to return Promise without waiting:

```hyperscript
def asyncOperation()
  async
    wait 2s
    return "Done"
  end
end

-- Returns Promise immediately
set promise to asyncOperation()
```

### Conditional Display

```hyperscript
-- Show/hide based on condition
<div _="show me when #search.value is not ''">
  Results will appear here
</div>

-- Filter elements
<li _="
  on keyup from #search
    if my textContent.toLowerCase() contains #search.value.toLowerCase()
      show me
    else
      hide me
    end
  end
">Item</li>
```

### Drag and Drop

```hyperscript
-- Draggable element
<div draggable="true" _="
  on dragstart
    set event.dataTransfer.effectAllowed to 'move'
    set event.dataTransfer.dropEffect to 'move'
    set event.dataTransfer.setData('text/plain', my id)
  end
">Drag me</div>

-- Drop target
<div _="
  on dragover
    halt the event
    set event.dataTransfer.dropEffect to 'move'
  end

  on drop
    halt the event
    set id to event.dataTransfer.getData('text/plain')
    append #${id} to me
  end
">Drop zone</div>
```

### Form Handling

```hyperscript
<form _="
  on submit
    halt the event
    make a FormData from me called formData
    fetch /api/submit with method:'POST', body:formData
    if it.ok
      put 'Success!' into #message
    else
      put 'Error!' into #message
    end
  end
">
```

### Table Row Filtering

```hyperscript
<input type="search" _="
  on keyup
    set query to my value.toLowerCase()
    repeat for row in <tbody tr/>
      if row.textContent.toLowerCase() contains query
        show row
      else
        hide row
      end
    end
  end
">
```

### Bulk Button Disabling

```hyperscript
<body _="
  on every htmx:beforeRequest
    if event.target matches <button.disable-on-request/>
      set @disabled to true on event.target
    end
  end

  on every htmx:afterOnLoad
    if event.target matches <button.disable-on-request/>
      set @disabled to false on event.target
    end
  end
">
```

### Checkbox Indeterminate State

```hyperscript
<input type="checkbox" _="
  on click
    js(me) me.indeterminate = true; end
  end
">
```

## Security

### XSS Prevention

Always escape third-party content:

```hyperscript
-- UNSAFE - never do this with user input
put userInput into #display

-- SAFE - escape HTML
set #display.textContent to userInput

-- Or use proper escaping
put escapeHTML(userInput) into #display
```

### Disabling Scripting

Prevent hyperscript execution in specific DOM areas:

```html
<!-- Disable for element and children -->
<div data-disable-scripting>
  <div _="on click log 'Will not execute'">Safe</div>
</div>
```

## Debugging

### Console Logging

```hyperscript
log "Debug message"
log "Variable x:", x
log "Multiple values:", x, y, z
```

### Beep Operator

Non-intrusive debugging that doesn't disrupt flow:

```hyperscript
set result to beep! expensiveComputation()
-- Logs: source, value, and type, then returns value
```

### Breakpoints

```hyperscript
on click
  set x to 10
  breakpoint  -- HDB debugger stops here (alpha feature)
  log x
end
```

## Complete Examples

### Click Counter

```html
<button _="
  init set @data-count to 0 end
  on click
    increment @data-count
    put 'Clicks: ' + @data-count into me
  end
">Clicks: 0</button>
```

### Auto-Save Form

```html
<textarea _="
  on keyup debounced at 1s
    fetch /api/autosave with method:'POST', body:{text: my value}
    if it.ok
      add .saved to #indicator
      wait 2s
      remove .saved from #indicator
    end
  end
">
```

### Infinite Scroll

```html
<div _="
  on intersection
    if intersecting
      fetch `/api/items?page=${@data-page}`
      put it before me
      increment @data-page
    end
  end
" data-page="1">Loading...</div>
```

### Accordion

```html
<div class="accordion">
  <div class="item">
    <div class="header" _="
      on click
        toggle .open on the closest .item
      end
    ">Section 1</div>
    <div class="content">Content 1</div>
  </div>
</div>

<style>
.item .content { display: none; }
.item.open .content { display: block; }
</style>
```

### Modal Dialog

```html
<button _="on click add .show to #modal">Open Modal</button>

<div id="modal" class="modal" _="
  on click
    if target is me
      remove .show from me
    end
  end

  on keydown[key is 'Escape'] from document
    remove .show from me
  end
">
  <div class="modal-content">
    <button _="on click remove .show from #modal">Close</button>
    <p>Modal content here</p>
  </div>
</div>
```

### Live Search

```html
<input type="search" _="
  on keyup
    if my value is ''
      put '' into #results
    else
      wait 300ms
      fetch `/api/search?q=${my value}` as json
      repeat for item in it
        make a <div>{item.name}</div> called div
        put div at end of #results
      end
    end
  end
">
<div id="results"></div>
```

### Toast Notifications

```html
<script type="text/hyperscript">
  def showToast(message)
    make a <div class="toast">{message}</div> called toast
    put toast at end of #toasts
    wait 3s
    transition toast's *opacity to 0
    settle
    remove toast
  end
</script>

<button _="on click call showToast('Hello!')">Show Toast</button>
<div id="toasts"></div>
```

## Best Practices

### 1. Locality of Behavior

Keep related code together on the element:

```hyperscript
<!-- GOOD: Behavior is local to element -->
<button _="on click add .active to me">Click me</button>

<!-- LESS IDEAL: Behavior is separated -->
<button id="myBtn">Click me</button>
<script>
  document.getElementById('myBtn').addEventListener('click', ...)
</script>
```

### 2. Use Natural Language

Leverage hyperscript's readable syntax:

```hyperscript
-- GOOD: Natural and readable
if element exists and element is not empty
  show element
end

-- WORKS: But less natural
if element != null && element.length > 0
  show element
end
```

### 3. Async Transparency

Let hyperscript handle Promises:

```hyperscript
-- GOOD: Linear, easy to read
fetch /api/data
put it into #result

-- LESS IDEAL: Manual Promise handling
async fetch /api/data
  .then(r => put r into #result)
```

### 4. Avoid Over-Engineering

Keep it simple:

```hyperscript
-- GOOD: Simple and direct
on click toggle .active on me

-- OVER-ENGINEERED: Unnecessary complexity
def toggleActive(el)
  if el.classList.contains('active')
    remove .active from el
  else
    add .active to el
  end
end
on click call toggleActive(me)
```

### 5. Use the Right Scope

Choose appropriate variable scope:

```hyperscript
-- Element scope for element-specific state
on click
  increment :clickCount
end

-- Global scope for app-wide state
on login
  set $currentUser to userData
end

-- Local scope for temporary values
on submit
  set local formData to my value
end
```

## Reference

### Command Quick Reference

| Command | Purpose | Example |
|---------|---------|---------|
| `add` | Add class/attribute | `add .active to me` |
| `remove` | Remove class/attribute/element | `remove .hidden from #el` |
| `toggle` | Toggle class/attribute | `toggle .open on me` |
| `set` | Set variable/property | `set x to 10` |
| `put` | Place content | `put "Hi" into me` |
| `wait` | Pause execution | `wait 2s` |
| `fetch` | HTTP request | `fetch /api/data` |
| `send` | Dispatch event | `send myEvent to #el` |
| `trigger` | Fire event | `trigger click on button` |
| `show` | Show element | `show #modal` |
| `hide` | Hide element | `hide .popover` |
| `transition` | Animate property | `transition *opacity to 0` |
| `log` | Console output | `log "Debug:", x` |
| `call` | Invoke function | `call myFunc()` |
| `repeat` | Loop | `repeat for x in items` |
| `if` | Conditional | `if x > 5 ... end` |
| `halt` | Stop event | `halt the event` |
| `measure` | Get dimensions | `measure my width` |
| `increment` | Increase value | `increment count` |
| `decrement` | Decrease value | `decrement @value` |
| `append` | Add to collection | `append item to list` |
| `make` | Create instance | `make a URL from path` |
| `throw` | Raise exception | `throw "Error"` |

### Expression Quick Reference

| Expression | Purpose | Example |
|------------|---------|---------|
| `#id` | ID selector | `#myElement` |
| `.class` | Class selector | `.active` |
| `<query/>` | Query selector | `<div.header/>` |
| `@attr` | Attribute | `@data-value` |
| `*prop` | Style property | `*opacity` |
| `my` | Current element property | `my innerHTML` |
| `it` | Last result | `put it into #el` |
| `event` | Event object | `event.target` |
| `the` | Readability helper | `the first <li/>` |
| `as Type` | Type conversion | `x as Int` |
| `of` | Property access | `length of array` |
| `in` | Scoped query | `<p/> in me` |
| `closest` | Ancestor search | `closest <div/>` |
| `next` | Next sibling | `next <li/>` |
| `previous` | Previous sibling | `previous <div/>` |
| `first` | First match | `first <option/>` |
| `last` | Last match | `last <item/>` |
| `is` | Comparison | `x is 10` |
| `matches` | CSS matching | `el matches <.active/>` |
| `contains` | Containment | `text contains "hello"` |
| `exists` | Null check | `value exists` |

### Event Modifiers

| Modifier | Purpose | Example |
|----------|---------|---------|
| `queue none` | Drop queued events | `on click queue none` |
| `queue all` | Process all events | `on click queue all` |
| `queue first` | Queue first only | `on click queue first` |
| `queue last` | Queue last (default) | `on click queue last` |
| `every` | Immediate execution | `on every click` |
| `[filter]` | Event filtering | `on click[button==0]` |
| `(params)` | Destructuring | `on keydown(key)` |

## Resources

- **Official Documentation**: https://hyperscript.org/docs/
- **Cookbook**: https://hyperscript.org/cookbook/
- **GitHub**: https://github.com/bigskysoftware/_hyperscript
- **Discord Community**: https://htmx.org/discord (includes hyperscript channel)

---

*This skills documentation provides comprehensive coverage of hyperscript syntax and patterns for use with Claude Code and AI-assisted development.*
