Unlike DOM events, component events don't _bubble_.
There are times when we need to listen to an event on some deeply nested component. The intermediate components must _forward_ event.
We can achieve this by using `createEventDispatcher`.

`App.svelte`
```svelte
<script>
	import Outer from './Outer.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}
</script>

<Outer on:message={handleMessage} />
```

`Outer.svelte`
```svelte
<script>
	import Inner from './Inner.svelte';
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function forward(event) {
		dispatch('message', event.detail);
	}
</script>

<Inner on:message={foward} />
```
Svelte allows us to write a very concise shorthand. We can just write:
`Outer.svelte`
```svelte
<script>
	import Inner from './Inner.svelte';
</script>

<Inner on:message />
```


`Inner.svelte`
```svelte
<script>
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
</script>

<button on:click={sayHello}>
	Click to say hello
</button>
```

So, the `on:<name>` directive allows only the parent component to receive events. They don't float to the top of the component hierarchy by themselves.