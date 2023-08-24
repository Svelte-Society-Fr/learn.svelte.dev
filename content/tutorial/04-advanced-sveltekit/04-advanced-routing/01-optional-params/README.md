---
title: Paramètres optionels
---

Dans le premier chapitre sur le [routing](/tutorial/pages), nous avons vu comment créer des route avec des [paramètres dynamiques](/tutorial/params).

Il est parfois utile de rendre un paramètre optionnel. Un exemple classique est lorsque vous utilisez un chemin pour déterminer la langue - `/fr/...`, `/de/...` et ainsi de suite — mais vous souhaitez également avoir une langue par défaut.

Pour faire cela, nous utilisons les crochets doubles. Renommez le dossier `[lang]` en `[[lang]]`.

L'application ne parvient plus à compiler, car les routes `src/routes/+page.svelte` et `src/routes/[[lang]]/+page.svelte` satisfont toutes les deux `/`. Supprimez la route `src/routes/+page.svelte` (il est possible que vous ayez besoin de recharger l'application pour annuler la page d'erreur).

Enfin, modifiez `src/routes/[[lang]]/+page.server.js` pour préciser la langue par défaut :

```js
/// file: src/routes/[[lang]]/+page.server.js
const greetings = {
	en: 'hello!',
	de: 'hallo!',
	fr: 'bonjour !'
};

export function load({ params }) {
	return {
		greeting: greetings[params.lang +++?? 'fr'+++]
	};
}
```
