---
title: Validateurs de paramètres
path: /colors/ff3e00
---

Pour éviter que le router reconnaisse des routes avec un paramètre invalide, vous pouvez préciser un _validateur_. Par exemple, vous pourriez vouloir une route comme `/colors/[value]` pour cibler les valeurs hexadécimales comme `/colors/ff3e00` mais pas les couleurs nommées comme `/colors/octarine`, ou tout autre valeur arbitraire.

Commencez par créer un nouveau fichier appelé `src/params/hex.js` et exportez-y une fonction `match` :

```js
/// file: src/params/hex.js
export function match(value) {
	return /^[0-9a-f]{6}$/.test(value);
}
```

Ensuite, pour utiliser le nouveau validateur, renommez `src/routes/colors/[color]` en `src/routes/colors/[color=hex]`.

À partir de maintenant, lorsque quelqu'un navigue vers cette route, SvelteKit va vérifier que `color` est une valeur `hex` valide. Si ce n'est pas le cas, SvelteKit va essayer de cibler d'autres routes, avant éventuellement de renvoyer une erreur 404.

> Les validateurs sont exécutés à la fois sur le serveur et sur le navigateur.
