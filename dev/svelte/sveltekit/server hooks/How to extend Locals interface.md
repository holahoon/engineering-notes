[Sveltekit documentation](https://kit.svelte.dev/docs/hooks#server-hooks) has a great explanation on how to use server hooks.

You add hooks in `src/hooks.server.ts`.

## Add custom data to the request
which is passed to handlers in `+server.ts` and server `load` functions:
Populate the `event.locals` objects:
```typescript
import type { Handle } from '@sveltejs/kit';

export const handle: Handle = async (input) => {
const { event, resolve } = input;

event.locals.user = 'dk';

return resolve(event);
};
```

💥 Notice how TypeScript complains: _Property 'user' does not exist on type 'Locals'_.

It's a very simple fix.
Head over to `app.d.ts` which declares the interface in the project. If no file exists, just create it in the root directory: `src/app.d.ts`.
Add the needed type in the file and done! 🥳
```typescript
// See https://kit.svelte.dev/docs/types#app
// for information about these interfaces

declare global {
	namespace App {
		// interface Error {}
		interface Locals {
			user: string;
		}
		// interface PageData {}
		// interface PageState {}
		// interface Platform {}
	}
}

export {};
```
