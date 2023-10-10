---
title: $env/static/private
---

Les variables d'environnement — comme les clés d'<span class="vo">[API](PUBLIC_SVELTE_SITE_URL/docs/development#api)</span> et les informations de connexion à une base de données — peuvent être ajoutées à un fichier `.env`, et seront rendues disponibles dans votre application.

> Vous pouvez aussi utiliser des fichiers `.env.local` ou `.env.[mode]` — voir la [documentation de Vite](https://vitejs.dev/guide/env-and-mode.html#env-files) (en anglais) pour plus d'informations. Faites bien attention à ajouter les fichiers contenant des informations sensibles à votre fichier `.gitignore` !
>
> Les variables d'environnement dans `process.env` sont aussi rendues disponibles via `$env/static/private`.

Dans cet exercice, nous voulons autoriser l'utilisateur ou l'utilisatrice à accéder au site si il ou elle connait le bon mot de passe, en utilisant une variable d'environnement.

D'abord, dans `.env`, ajoutez une nouvelle variable d'environnement :

```env
/// file: .env
PASSPHRASE=+++"sesame ouvre-toi"+++
```

Ouvrez `src/routes/+page.server.js`. Importez `PASSPHRASE` depuis `$env/static/private` et servez-vous en dans l'[action de formulaire](/tutorial/the-form-element) :

```js
/// file: src/routes/+page.server.js
import { redirect, fail } from '@sveltejs/kit';
+++import { PASSPHRASE } from '$env/static/private';+++

export function load({ cookies }) {
	if (cookies.get('allowed')) {
		throw redirect(307, '/welcome');
	}
}

export const actions = {
	default: async ({ request, cookies }) => {
		const data = await request.formData();

		if (data.get('passphrase') === +++PASSPHRASE+++) {
			cookies.set('allowed', 'true', {
				path: '/'
			});

			throw redirect(303, '/welcome');
		}

		return fail(403, {
			incorrect: true
		});
	}
};
```

Le site est maintenant accessible à toute personne connaissant le mot de passe.

## Garder des secrets

Il est primordial que de la donnée sensible ne se retrouve pas accidentellement envoyée au navigateur, où elle serait très facilement volée par des hackers et autres canailles.

SvelteKit permet d'empêcher cela très facilement. Regardez ce qu'il se produit si nous essayons d'importer `PASSPHRASE` dans `src/routes/+page.svelte` :

```svelte
/// file: src/routes/+page.svelte
<script>
	+++import { PASSPHRASE } from '$env/static/private';+++
	export let form;
</script>
```

Une erreur apparaît, nous informant que `$env/static/private` ne peut pas être importé dans du code qui sera exécuté sur le client. Il peut uniquement être importé dans des modules qui seront exécutés sur le serveur :

- `+page.server.js`
- `+layout.server.js`
- `+server.js`
- tout module se terminant par `.server.js`
- tout module dans `src/lib/server`

De plus, ces modules peuvent uniquement être importés par d'_autres_ modules de serveur.

## Statique ou dynamique

Le `static` dans `$env/static/private` indique que ces valeurs sont connues au moment de la compilation, et peuvent être _remplacées de manière statique_. Cela permet des optimisations utiles :

```js
/// no-file
import { FEATURE_FLAG_X } from '$env/static/private';

if (FEATURE_FLAG_X === 'enabled') {
	// le code écrit ici sera supprimé du build
	// si FEATURE_FLAG_X n'est pas activé
}
```

Dans certains cas, vous pourriez avoir besoin d'utiliser des variables d'environnement qui sont _dynamiques_ — en d'autres termes, inconnues tant que l'application n'a pas été exécutée. Nous traiterons ce cas dans le prochain exercice.
