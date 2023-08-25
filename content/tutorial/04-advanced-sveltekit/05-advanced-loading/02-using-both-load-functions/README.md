---
title: Utiliser les deux fonctions load
---

Occasionnellement, vous pourriez avoir besoin d'utiliser ensemble une fonction `load` de serveur et une fonction `load` universelle. Par exemple, vous pourriez vouloir obtenir de la donnée du serveur, mais aussi obtenir une valeur qui ne peut pas être sérialisée.

Dans cet exemple, nous voulons renvoyer de la fonction `load` un composant différent selon que la donnée obtenue depuis `src/routes/+page.server.js` est `cool` ou non.

Nous pouvons accéder à la donnée du serveur dans `src/routes/+page.js` via la propriété `data` :

```js
/// file: src/routes/+page.js
export async function load(+++{ data }+++) {
	const module = +++data.cool+++
		? await import('./CoolComponent.svelte')
		: await import('./BoringComponent.svelte');

	return {
		component: module.default,
		message: +++data.message+++
	};
}
```

> Notez que la donnée n'est pas fusionnée — nous devons explicitement renvoyer `message` depuis la fonction `load` universelle.
