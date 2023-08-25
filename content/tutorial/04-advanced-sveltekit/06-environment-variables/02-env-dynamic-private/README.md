---
title: $env/dynamic/private
---

Si vous avez besoin de lire les valeurs des variables d'environnement lorsque l'application en cours d'exécution, plutôt que lorsque l'application est compilée, vous pouvez utiliser `$env/dynamic/private` à la place de `$env/static/private` :

```js
/// file: src/routes/+page.server.js
import { redirect, fail } from '@sveltejs/kit';
import { +++env+++ } from '$env/+++dynamic+++/private';

export function load({ cookies }) {
	if (cookies.get('allowed')) {
		throw redirect(307, '/welcome');
	}
}

export const actions = {
	default: async ({ request, cookies }) => {
		const data = await request.formData();

		if (data.get('passphrase') === +++env.+++PASSPHRASE) {
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
