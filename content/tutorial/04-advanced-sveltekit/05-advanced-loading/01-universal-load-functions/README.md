---
title: Fonctions load universelles
---

Dans le [précédent chapitre sur le chargement](page-data), nous avons chargé de la donnée depuis le serveur en utilisant les fichiers `+page.server.js` et `+layout.server.js`. Cette méthode est très pratique si vous avez besoin d'accéder directement à une base de données, ou de lire des cookies.

Parfois, charger de la donnée depuis le serveur lors d'une navigation côté client n'a pas vraiment de sens. Par exemple :

- Vous chargez de la donnée depuis une <span class="vo">[API](PUBLIC_SVELTE_SITE_URL/docs/development#api)</span> externe
- Vous voulez utiliser de la donnée en mémoire du navigateur, si disponible
- Vous voulez retarder la navigation jusqu'à ce qu'une image ait été chargée, pour éviter de la faire apparaître brusquement
- Vous avez besoin de renvoyer quelque chose depuis `load` qui ne peut pas être sérialisé (SvelteKit utilise [devalue](https://github.com/Rich-Harris/devalue) pour transformer la donnée du serveur en <span class="vo">[JSON](PUBLIC_SVELTE_SITE_URL/docs/web#json)</span>), comme un composant ou un <span class="vo">[store](PUBLIC_SVELTE_SITE_URL/docs/sveltejs#store)</span>

Dans cet exercice, nous avons à faire au dernier cas. Les fonction `load` de serveur dans `src/routes/red/+page.server.js`, `src/routes/green/+page.server.js` et `src/routes/blue/+page.server.js` renvoient un constructeur `component`, qui ne peut pas être sérialisé comme de la donnée. Si vous naviguez vers `/red`, `/green` ou `/blue`, vous verrez apparaître dans le terminal l'erreur "Data returned from `load` ... is not serializable" (_La donnée ... renvoyée par `load` n'est pas sérialisable_).

Pour transformer les fonctions `load` de serveur en fonctions `load` universelles, renommez chaque fichier `+page.server.js` en `+page.js`. Désormais, les fonctions `load` seront exécutées sur le serveur lors du rendu côté serveur, mais elles seront également exécutées dans le navigateur lorsque l'application est hydratée, ou lorsque l'utilisateur ou l'utilisatrice déclenche une navigation côté client.

Nous pouvons maintenant utiliser la valeur `component` renvoyée par ces fonctions `load` comme n'importe quelle autre valeur, même dans `src/routes/+layout.svelte` :

```svelte
/// file: src/routes/+layout.svelte
<nav
	class:has-color={!!$page.data.color}
	style:background={$page.data.color ?? 'var(--bg-2)'}
>
	<a href="/">accueil</a>
	<a href="/red">rouge</a>
	<a href="/green">vert</a>
	<a href="/blue">bleu</a>

+++	{#if $page.data.component}
		<svelte:component this={$page.data.component} />
	{/if}+++
</nav>
```

Rendez-vous dans la [documentation](PUBLIC_KIT_SITE_URL/docs/load#universal-vs-server) pour en apprendre plus sur la différence entre une fonction `load` de serveur et une fonction `load` universelle, ainsi que les cas d'usage de chacune.
