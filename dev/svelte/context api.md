Context api sounds very familiar! Ah, the good old React Context Api!.
Svelte's context api is very similar to React's, but with one difference. It is _NOT_ reactive. It is set once when the component mounts, and will not be updated again.
Context in Svelte does one thing: pass data from a parent component to any children (_not necessarily direct children_).

Here's an example:
`+page.svelte`
```vue
<script lang="ts">
	import Parent from '@components/context-api/parent.svelte'
	import Child from '@components/context-api/child.svelte'
</script>

<h1>Context API</h1>

<Parent>
	<Child />
</Parent>
```

`parent.svelte`
```vue
<script lang="ts">
	import { setContext } from 'svelte'
	import type { ContextExample } from '$lib/stores/type'

	let text = 'This Is Svelte Context API'

	setContext<ContextExample>('context-example', {
		getText: () => text
	})
</script>

<div>
	<slot />
</div>
```

`child.svelte`
```vue
<script lang="ts">
	import type { ContextExample } from '$lib/stores/type'
	import { getContext } from 'svelte'

	const { getText } = getContext<ContextExample>('context-example')
	const text = getText()
</script>

<p>{text}</p>
```

`types.ts`
```typescript
export interface ContextExample {
	getText: () => string
}
```

The `text` value is just set once and done. It can't be updated or changed.
But, what if I want to make my context reactive?
Well, Svelte offers `store` right? We can combine these two 🚀

## Combine Context + Store 🥳

`count-store.ts`
```typescript
import { getContext, setContext } from 'svelte'
import { writable, type Writable } from 'svelte/store'

type Count = number
type Context = Writable<number>

const COUNT_CONTEXT = 'count-context' as const

export function setCount() {
	const count = writable<Count>(0)
	setContext<Context>(COUNT_CONTEXT, count)
}

export function getCount() {
	return getContext<Context>(COUNT_CONTEXT)
}
```
Notice how I'm declaring the `count` state inside the `setCount()` function. This is to have a scoped state, instead of having a global state.

`+page.svelte`
```vue
<script lang="ts">
	import { Child, Parent, GrandParent } from '@components/context-store/index'
</script>

<!-- one -->
<GrandParent>
	<Parent>
		<Child />
	</Parent>
</GrandParent>

<!-- two -->
<GrandParent>
	<Parent>
		<Child />
	</Parent>
</GrandParent>
```
Since the `count` state declared in a lexical scope, Both `one` and `two` will have different state values!

`grand-parent.svelte`
```vue
<script lang="ts">

import { setCount, getCount } from '$lib/stores/count-store'

setCount()
const count = getCount()

function updateCount(event: WheelEvent) {
	event.deltaY < 0 ? ($count += 1) : ($count -= 1)
}
</script>

<div on:wheel={updateCount} class="container">
	<p>GrandParent: {$count}</p>
	<slot />
</div>
```

`parent.svelte`
```vue
<script lang="ts">
	import { getCount } from '$lib/stores/count-store'
	
	const count = getCount()
</script>

<div class="container">
	<p>Parent: {$count}</p>
	<slot />
</div>
```

`child.svelte`
```vue
<script lang="ts">
	import { getCount } from '$lib/stores/count-store'
	
	const count = getCount()
</script>

<div class="container">
	<p>Child: {$count}</p>
	<slot />
</div>
```

Now our context is reactive 👏