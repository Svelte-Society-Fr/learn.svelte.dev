---
title: Dépendances personnalisées
path: /Europe/London
---

Appeler `fetch(url)` dans une fonction `load` enregistre `url` comme dépendance de la fonction. Mais ce n'est pas toujours approprié d'utiliser `fetch`, et dans ce cas vous pouvez spécifier une dépendance manuellement avec la fonction [`depends(url)`](KIT_SITE_URL/docs/load#invalidation-manual-invalidation).

Puisque toute chaîne de caractères commançant par `[a-z]+:` est une URL valide, nous pouvons créer des clés d'invalidation personnalisées, comme `data:now`.

Modifiez `src/routes/+layout.js` pour renvoyer une valeur directement plutôt que de faire un appel `fetch`, et exécutez la fonction `depends` :

```js
/// file: src/routes/+layout.js
export async function load({ +++depends+++ }) {
	+++depends('data:now');+++

	return {
		now: +++Date.now()+++
	};
}
```

Maintenant, modifiez l'appel à `invalidate` dans `src/routes/[...timezone]/+page.svelte` :

```svelte
/// file: src/routes/[...timezone]/+page.svelte
<script>
	import { onMount } from 'svelte';
	import { invalidate } from '$app/navigation';

	export let data;

	onMount(() => {
		const interval = setInterval(() => {
			invalidate(+++'data:now'+++);
		}, 1000);

		return () => {
			clearInterval(interval);
		};
	});
</script>
```
