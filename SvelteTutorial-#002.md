# BASIC SVELTE

Before reading this file, make sure you have read the "Svelte tutorial.pdf" file contained within this repository.
It consists of the first part of my hand written svelte notes and covers the following :
- [x] Introduction
- [x] Reactivity
- [x] Props
- [x] Logic

For the rest of this course, I have switched from hand written notes to github markdown which will cover the rest of the topics included in basic svelte:
- [ ] Events
- [ ] Bindings
- [ ] Lifecycles
- [ ] Stores

##  EVENTS
### DOM events

Within the hand written notes we used an event hander called " on: click".
You can listen to any event in the DOM usin the " on: " directive

```javascript
<script>
let m = { x : 0 , y : 0 };
function onmove (event) {
m.x = event.ClientX;
m.y = event.ClientY;
}
</script>
<div on:pointermove={onmove}>
The pointer is at {m.x} x {m.y}
</div>
```

### Inline Handlers

You can also declare events inline in the html 

```javascript
<script>
	let m = { x: 0, y: 0 };
</script>

<div
	on:pointermove={(e) => {
		m = { x: e.clientX, y: e.clientY };
	}}
>
	The pointer is at {m.x} x {m.y}
</div>
```

### Event Modifiers

You can modify events using the " | " symbol 

```html
<button on:click|once={() => alert('I am clicked')}>
	Click me
</button>
```

### Component event & Event Forwarding

There's an issue here, I'm getting an error that createEventDispatcher has been depricated
which is necessary for the subject

## BINDINGS
### Text Inputs

You can bind the value of the text inputs so that it gets updated,
This is simplified instead of using event handlers, with the following code,
the value of name is updated in either changing its value when declaring the variable, 
or when you input a text

```javascript,html
<script>
	let name = 'world';
</script>

<input bind:value={name} />

<h1>Hello {name}!</h1>
```

### Numeric Inputs

You can also bind numeric inputs as follows: 

```javascript
<script>
	let a = 1;
	let b = 2;
</script>

<label>
	<input type="number" bind:value={a} min="0" max="10" />
	<input type="range" bind:value={a} min="0" max="10" />
</label>

<label>
	<input type="number" bind:value={b} min="0" max="10" />
	<input type="range" bind:value={b} min="0" max="10" />
</label>

<p>{a} + {b} = {a + b}</p>
```

### Checkbox inputs

In checkbox input, we code the "bind:" keyword in the checked status, consider the following code

```javascript
<script>
	let yes = false;
</script>

<label>
	<input type="checkbox" bind:checked={yes} />
	Yes! Send me regular email spam
</label>

{#if yes}
	<p>
		Thank you. We will bombard your inbox and sell
		your personal details.
	</p>
{:else}
	<p>
		You must opt in to continue. If you're not
		paying, you're the product.
	</p>
{/if}

<button disabled={!yes}>Subscribe</button>
```

### Selected bindings

Here, we can also use " bind:value " with selected elements

```javascript
<script>
	let questions = [
		{
			id: 1,
			text: `Where did you go to school?`
		},
		{
			id: 2,
			text: `What is your mother's name?`
		},
		{
			id: 3,
			text: `What is another personal fact that an attacker could easily find with Google?`
		}
	];

	let selected;

	let answer = '';

	function handleSubmit() {
		alert(
			`answered question ${selected.id} (${selected.text}) with "${answer}"`
		);
	}
</script>

<h2>Insecurity questions</h2>

<form on:submit|preventDefault={handleSubmit}>
	<select
		bind:value={selected}
		on:change={() => (answer = '')}
	>
		{#each questions as question}
			<option value={question}>
				{question.text}
			</option>
		{/each}
	</select>

	<input bind:value={answer} />

	<button disabled={!answer} type="submit">
		Submit
	</button>
</form>

<p>
	selected question {selected
		? selected.id
		: '[waiting...]'}
</p>
```

### Group inputs

If you have multiple radio or checked inputs, you can bind them using " bind:group " along with the value, as shown below:

```javascript
<script>
	let questions = [
		{
			id: 1,
			text: `Where did you go to school?`
		},
		{
			id: 2,
			text: `What is your mother's name?`
		},
		{
			id: 3,
			text: `What is another personal fact that an attacker could easily find with Google?`
		}
	];

	let selected;

	let answer = '';

	function handleSubmit() {
		alert(
			`answered question ${selected.id} (${selected.text}) with "${answer}"`
		);
	}
</script>

<h2>Insecurity questions</h2>

<form on:submit|preventDefault={handleSubmit}>
	<select
		bind:value={selected}
		on:change={() => (answer = '')}
	>
		{#each questions as question}
			<option value={question}>
				{question.text}
			</option>
		{/each}
	</select>

	<input bind:value={answer} />

	<button disabled={!answer} type="submit">
		Submit
	</button>
</form>

<p>
	selected question {selected
		? selected.id
		: '[waiting...]'}
</p>
```

### Select multiple
A select element can have a multiple attribute, in which case it will populate an array rather than selecting a single value. This enables multiple selection of items with binded values as shown in the code example below:

```javascript

<script>
	let scoops = 1;
	let flavours = [];

	const formatter = new Intl.ListFormat('en', { style: 'long', type: 'conjunction' });
</script>

<h2>Size</h2>

{#each [1, 2, 3] as number}
	<label>
		<input
			type="radio"
			name="scoops"
			value={number}
			bind:group={scoops}
		/>

		{number} {number === 1 ? 'scoop' : 'scoops'}
	</label>
{/each}

<h2>Flavours</h2>

<select multiple bind:value={flavours}>
	{#each ['cookies and cream', 'mint choc chip', 'raspberry ripple'] as flavour}
		<option>{flavour}</option>
	{/each}
</select>

{#if flavours.length === 0}
	<p>Please select at least one flavour</p>
{:else if flavours.length > scoops}
	<p>Can't order more flavours than scoops!</p>
{:else}
	<p>
		You ordered {scoops} {scoops === 1 ? 'scoop' : 'scoops'}
		of {formatter.format(flavours)}
	</p>
{/if}
```

### Text Area inputs

The <textarea> element behaves similarly to a text input in Svelte — use bind:value:

```javascript
<script>
	import { marked } from 'marked';
	let value = `Some words are *italic*, some are **bold**\n\n- lists\n- are\n- cool`;
</script>

<div class="grid">
	input
	<textarea bind:value></textarea>

	output
	<div>{@html marked(value)}</div>
</div>

```


## LIFECYCLE

### onMount

Every component has a lifecycle that starts when it is created, and ends when it is destroyed. There are a handful of functions that allow you to run code at key moments during that lifecycle. The one you'll use most frequently is onMount, which runs after the component is first rendered to the DOM.

### beforeUpdate and afterUpdate

The beforeUpdate function schedules work to happen immediately before the DOM is updated. afterUpdate is its counterpart, used for running code once the DOM is in sync with your data.

Together, they're useful for doing things imperatively that are difficult to achieve in a purely state-driven way, like updating the scroll position of an element 

### tick

When you update component state in Svelte, it doesn't update the DOM immediately
The tick function is unlike other lifecycle functions in that you can call it any time, not just when the component first initialises. It returns a promise that resolves as soon as any pending state changes have been applied to the DOM (or immediately, if there are no pending state changes).

## STORES

### Writable stores
 
A store is simply an object with a subscribe method that allows interested parties to be notified whenever the store value changes. 
Not all application state belongs inside your application's component hierarchy. Sometimes, you'll have values that need to be accessed by multiple unrelated components, or by a regular JavaScript module.

consider the example below,

The code in stores.js has the definition of count. It's a writable store, which means it has set and update methods in addition to subscribe

#### src/App.svelte
```javascript
<script>
	import { count } from './stores.js';
	import Incrementer from './Incrementer.svelte';
	import Decrementer from './Decrementer.svelte';
	import Resetter from './Resetter.svelte';

	let count_value;

	count.subscribe((value) => {
		count_value = value;
	});
</script>

<h1>The count is {count_value}</h1>

<Incrementer />
<Decrementer />
<Resetter />
```

#### src/Decrementer.svelte
```javascript
<script>
	import { count } from './stores.js';

	function decrement() {
		count.update((n) => n - 1);
	}
</script>

<button on:click={decrement}>
	-
</button>
```

#### src/Incrementer.svelte
```javascript
<script>
	import { count } from './stores.js';

	function increment() {
		count.update((n) => n + 1);
	}
</script>

<button on:click={increment}>
	+
</button>
```

#### src/Resetter.svelte
```javascript
<script>
	import { count } from './stores.js';

	function reset() {
		count.set(0);
	}
</script>

<button on:click={reset}>
	reset
</button>
```

#### src/stores.js
```javascript
import { writable } from 'svelte/store';

export const count = writable(0);
```

### Auto-subscriptions

Instead on using complex methods to unsubscribe like importing on destroy,you can reference a store value by prefixing the store name with $.
Auto-subscription only works with store variables that are declared (or imported) at the top-level scope of a component.
Any name beginning with $ is assumed to refer to a store value. It's effectively a reserved character — Svelte will prevent you from declaring your own variables with a $ prefix.
We can update the code in App.svelte as follows: 

```javascript
<script>
	import { count } from './stores.js';
	import Incrementer from './Incrementer.svelte';
	import Decrementer from './Decrementer.svelte';
	import Resetter from './Resetter.svelte';
</script>

<h1>The count is {$count}</h1>

<Incrementer />
<Decrementer />
<Resetter />
```

### Readable stores

Some stores are used for times when data from the user is unnecessary.

The first argument to readable is an initial value, which can be null or undefined if you don't have one yet. The second argument is a start function that takes a set callback and returns a stop function. The start function is called when the store gets its first subscriber; stop is called when the last subscriber unsubscribes.

#### App.svelte
```javascript
<script>
	import { time } from './stores.js';

	const formatter = new Intl.DateTimeFormat(
		'en',
		{
			hour12: true,
			hour: 'numeric',
			minute: '2-digit',
			second: '2-digit'
		}
	);
</script>

<h1>The time is {formatter.format($time)}</h1>
```

#### stores.js
```javascript
import { readable } from 'svelte/store';

export const time = readable(new Date(), function start(set) {
	const interval = setInterval(() => {
		set(new Date());
	}, 1000);

	return function stop() {
		clearInterval(interval);
	};
});
```

### Derived stores

You can create a store whose value is based on the value of one or more other stores with derived. Building on our previous example, we can create a store that derives the time the page has been open:

#### stores.js
```javascript
export const elapsed = derived(
	time,
	($time) => Math.round(($time - start) / 1000)
);
```

import elapsed to app.svelte and create a p tag to display it


### Custom stores

As long as an object correctly implements the subscribe method, it's a store. Beyond that, anything goes. It's very easy, therefore, to create custom stores with domain-specific logic.

For example, the count store from our earlier example could include increment, decrement and reset methods and avoid exposing set and update

#### stores.js
```javascript
import { writable } from 'svelte/store';

function createCount() {
	const { subscribe, set, update } = writable(0);

	return {
		subscribe,
		increment: () => update((n) => n + 1),
		decrement: () => update((n) => n - 1),
		reset: () => set(0)
	};
}

export const count = createCount();
```

#### App.svelte
```javascript
<script>
	import { count } from './stores.js';
</script>

<h1>The count is {$count}</h1>

<button on:click={count.increment}>+</button>
<button on:click={count.decrement}>-</button>
<button on:click={count.reset}>reset</button>

```

### Store bindings
If a store is writable — i.e. it has a set method — you can bind to its value, just as you can bind to local component state.

In this example we're exporting a writable store name and a derived store greeting

#### App.svelte
```javascript
<script>
	import { name, greeting } from './stores.js';
</script>

<h1>{$greeting}</h1>
<input bind:value={$name} />

<button on:click={() => $name += '!'}>
	Add exclamation mark!
</button>

```

#### stores.js
```javascript
import { writable, derived } from 'svelte/store';

export const name = writable('world');

export const greeting = derived(name, ($name) => `Hello ${$name}!`);

```

Changing the input value will now update name and all its dependents.

We can also assign directly to store values inside a component. Add an on:click event handler to update name.

The $name += '!' assignment is equivalent to name.set($name + '!').



