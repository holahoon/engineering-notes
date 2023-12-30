There are times when we need to use client components in our Next.js application. Here's a good way to implement React's Context API 🥳

Let's suppose we have a fetch class (it just fetches from `dummjson` api 🙃). _I was too lazy to put in types and catching errors XD_
`lib/api.ts`
```typescript
class DataDummyJsonApi {
	constructor() {}

	async getProducts(): Promise<unknown> {
		const res = await fetch("https://dummyjson.com/products");
		return await res.json();
	}

	async getProduct(id: number): Promise<unknown> {
		const res = await fetch(`https://dummyjson.com/products/${id}`);
		return await res.json();
	}
}

export const dummyJsonApi = new DataDummyJsonApi();
```

Here's a simple page component:
`page.tsx`
```tsx
"use client";

import { dummyJsonApi } from "@/app/lib/api";
import { useDummyJsonApi } from "./layout";

export default function Page() {
	return (
		<div>
			<button
				onClick={async () => {
					const data = await dummyJsonApi.getProducts();
					console.log(data);
			}}>
				🦮 Fetch!
			</button>
		</div>
	);
}
```

We can easily get tempted to just call `dummyJsonApi` and fetch right away.

Well, this is okay... but, we can easily get into problems when writing a component unit test. We need to figure out a way to mock a fake api for the click event. It's just simply harder to write test codes.
Let's refactor it!

## Dependency Injection to the rescue 🚀

Let's create a `layout.tsx` and use React Context API

`layout.tsx`
```tsx
"use client";

import { dummyJsonApi } from "@/app/lib/api";
import { type PropsWithChildren, createContext, useContext } from "react";

export type DummyJsonApiProvider = {
	getProducts(): Promise<unknown>;
};

const DummyJsonApiContext = createContext<DummyJsonApiProvider | null>(null);

export function useDummyJsonApi() {
	return useContext(DummyJsonApiContext);
}

export default function Layout(props: PropsWithChildren) {
	const { children } = props;
	return (
		<DummyJsonApiContext.Provider value={dummyJsonApi}>
			{children}	
		</DummyJsonApiContext.Provider>
	);
}
```
Wrap the children components with its appropriate context.

`page.tsx`
```tsx
"use client";

import { useDummyJsonApi } from "./layout";

export default function Page() {
	const apiContext = useDummyJsonApi();
	
	if (!apiContext) return null;
	return (
		<div>
			<button
				onClick={async () => {
					const data = await dummyJsonApi.getProducts();
					console.log(data);
			}}>
				🦮 Fetch!
			</button>
		</div>
	);
}
```
Since we have context inside our `layout.tsx`, We can simply call the custom context hook 

`page.test.tsx`
```tsx
import {DummyJsonApiContext} from './layout'
import {SomePage} from './page'

describe("page component", () => {
	it("should do stuff", () => {
		render(
			<DummyJsonApiContext.Provider value={{
				async getProducts(){
					return [{id: 1, otherThings: {...}}]
				}
			}}>
					<SomePage />
			</DummyJsonApiContext.Provider>
		)
	})
})
```
See how simple our unit test can be? just pass in some fake dummy data inside the provider and we can focus on writing unit tests for our page component! Of course we can refactor this test code as well =)
