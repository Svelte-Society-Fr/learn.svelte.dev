---
title: invalidateAll
path: /Europe/London
---

En dernier recours, il y a l'option nucléaire — `invalidateAll()`. Cela va ré-exécuter toutes les fonctions `load` de la page courante, quelque que soient ses dépendances.

Modifiez `src/routes/[...timezone]/+page.svelte` de l'exercice précédent :

```svelte
/// file: src/routes/[...timezone]/+page.svelte
<script>
	import { onMount } from 'svelte';
	import { +++invalidateAll+++ } from '$app/navigation';

	export let data;

	onMount(() => {
		const interval = setInterval(() => {
			+++invalidateAll();+++
		}, 1000);

		return () => {
			clearInterval(interval);
		};
	});
</script>
```

L'exécution de `depends` dans `src/routes/+layout.js` n'est alors plus nécessaire :

```js
/// file: src/routes/+layout.js
export async function load(---{ depends }---) {
	---depends('data:now');---

	return {
		now: Date.now()
	};
}
```

> `invalidate(() => true)` et `invalidateAll` ne font _pas_ la même chose. `invalidateAll` ré-exécute également les fonctions `load` qui n'ont pas de dépendance envers une `url`, ce que `invalidate(() => true)` ne fait pas.
