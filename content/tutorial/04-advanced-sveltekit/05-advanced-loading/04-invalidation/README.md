---
title: Invalidation
path: /Europe/London
---

Lorsque l'utilisateur ou l'utilisatrice navigue d'une page à l'autre, SvelteKit appelle vos différentes fonctions `load` uniquement s'il pense que quelque chose a changé.

Dans cet exemple, la navigation entre fuseaux horaires provoque la ré-exécution de la fonction `load` de `src/routes/[...timezone]/+page.js` parce que `params.timezone` a changé. Mais la fonction `load` dans `src/routes/+layout.js` n'est _pas_ ré-exécutée, car du point de vue de SvelteKit rien dans cette fonction `load` n'est concerné par cette navigation.

Nous pouvons régler cela en invalidant cette fonction manuellement grâce à la fonction [`invalidate(...)`](PUBLIC_KIT_SITE_URL/docs/modules#$app-navigation-invalidate), qui prend une URL en paramètre et ré-exécute toutes les fonctions `load` qui en dépendent. Puisque la fonction `load` de `src/routes/+layout.js` appelle `fetch('/api/now')`, elle dépend de `/api/now`.

Dans `src/routes/[...timezone]/+page.svelte`, ajoutez un <span class="vo">[callback](PUBLIC_SVELTE_SITE_URL/docs/development#callback)</span> `onMount` qui va exécuter `invalidate('/api/now')` une fois par seconde :

```svelte
/// file: src/routes/[...timezone]/+page.svelte
<script>
	+++import { onMount } from 'svelte';+++
	+++import { invalidate } from '$app/navigation';+++

	export let data;

+++	onMount(() => {
		const interval = setInterval(() => {
			invalidate('/api/now');
		}, 1000);

		return () => {
			clearInterval(interval);
		};
	});+++
</script>

<h1>
	{new Intl.DateTimeFormat([], {
		timeStyle: 'full',
		timeZone: data.timezone
	}).format(new Date(data.now))}
</h1>
```

> Vous pouvez aussi passer une fonction à `invalidate`, dans le cas où vous souhaiteriez invalider votre page en fonction d'un motif d'URL et non d'URLs spécifiques.
