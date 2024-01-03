Typically, data flows from top to bottom. If you pass an `input` state from one component to another and update it with a handler, it typically looks like:
```vue
<script lang="ts">
  import Input from './input.svelte'
  import Output from './output.svelte'

  let input = ''
</script>

<Input updateInput={(event) => input = event.target.value} />
	<Output {input} />
</script>
```


Using the `bind:property` directive, we can also let it flow the other way (child -> parent)
`+page.svelte`
```vue
<script lang="ts">
  import Input from './input.svelte'
  import Output from './output.svelte'

  let input = ''
</script>

<Input bind:input />
<Output {input} />
```

`input.svelte`
```vue
<script lang="ts">
  export let input: string
</script>

<input type="text" bind:value={input} />
```

`output.svelte`
```vue
<script lang="ts">
  export let input: string
</script>

<p>{input}</p>
```

But, be careful of abusing `bind` because the more you pass it around, it's going to be hard to keep track of the state and what changes it.
