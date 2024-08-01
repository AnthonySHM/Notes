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
A select element can have a multiple attribute, in which case it will populate an array rather than selecting a single value.

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

<style>
	.grid {
		display: grid;
		grid-template-columns: 5em 1fr;
		grid-template-rows: 1fr 1fr;
		grid-gap: 1em;
		height: 100%;
	}

	textarea {
		flex: 1;
		resize: none;
	}
</style>
```
