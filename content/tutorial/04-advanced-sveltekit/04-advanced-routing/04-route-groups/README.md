---
title: Routes groupées
---

Comme nous l'avons vu dans l'[introduction sur le routing](/tutorial/layouts), les <span class="vo">[layouts](SVELTE_SITE_URL/docs/web#layout)</span> permettent de partager de l'interface et du chargement de données entre différentes routes.

Il est parfois pratique d'utiliser des <span class="vo">[layouts](SVELTE_SITE_URL/docs/web#layout)</span> sans affecter la route — par exemple, vous pourriez avoir besoin que vos routes `/app` et `/account` soient derrière une authentification, tandis que votre page `/about` reste accessible par tout le monde. Vous pouvez faire cela avec un _groupe de routes_, qui est un dossier entre parenthèses.

Créez un groupe `(authed)` en renommant `account` en `(authed)/account` puis renommez `app` en `(authed)/app`.

Nous pouvons maintenant contrôler l'accès à ces routes en créant `src/routes/(authed)/+layout.server.js` :

```js
/// file: src/routes/(authed)/+layout.server.js
import { redirect } from '@sveltejs/kit';

export function load({ cookies, url }) {
	if (!cookies.get('logged_in')) {
		throw redirect(303, `/login?redirectTo=${url.pathname}`);
	}
}
```

Si vous essayez de visiter ces pages, vous serez redirigés vers la route `/login`, qui contient une action de formulaire dans `src/routes/login/+page.server.js` permettant de définir le cookie `logged_in`.

Nous pouvez aussi ajouter de l'interface pour ces deux routes en ajoutant un fichier `src/routes/(authed)/+layout.svelte` :

```svelte
/// file: src/routes/(authed)/+layout.svelte
<slot />

<form method="POST" action="/logout">
	<button>se déconnecter</button>
</form>
```
