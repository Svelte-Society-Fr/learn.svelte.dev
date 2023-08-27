---
title: Utiliser la donnée parente
---

Comme nous l'avons vu dans l'introduction à la [donnée des layouts](/tutorial/layout-data), les composants `+page.svelte` et `+layout.svelte` ont accès à tout ce qui est renvoyé par leurs fonctions `load` parentes.

Il est parfois utile que les fonctions `load` elles-mêmes aient accès à la donnée de leurs parents. Cela peut être fait avec `await parent()`.

Pour montrer comment cela fonctionne, nous allons additionner deux nombres qui viennent de fonctions `load` différentes. D'abord, renvoyez de la donnée depuis `src/routes/+layout.server.js` :

```js
/// file: src/routes/+layout.server.js
export function load() {
	return { +++a: 1+++ };
}
```

Puis, récupérer cette donnée dans `src/routes/sum/+layout.js` :

```js
/// file: src/routes/sum/+layout.js
export async function load(+++{ parent }+++) {
	+++const { a } = await parent();+++
	return { +++b: a + 1+++ };
}
```

> Il est important que noter qu'une fonction `load` [universelle](/tutorial/universal-load-functions) peut accéder à de la donnée parente depuis une fonction `load` de _serveur_. L'inverse n'est pas possible — une fonction `load` de serveur peut uniquement accéder à la donnée parente d'une autre fonction `load` de serveur.

Enfin, dans `src/routes/sum/+page.js`, récupérer la donnée parente depuis les deux fonctions `load` :

```js
/// file: src/routes/sum/+page.js
export async function load(+++{ parent }+++) {
	+++const { a, b } = await parent();+++
	return { +++c: a + b+++ };
}
```

> Faites attention à ne pas introduire de cascades de chargement qui ralentiraient le chargement de vos pages lorsque vous utilisez `await parent()`. Si vous pouvez `fetch` une autre donnée qui ne dépend pas de la donnée parent, faites-le en premier.
