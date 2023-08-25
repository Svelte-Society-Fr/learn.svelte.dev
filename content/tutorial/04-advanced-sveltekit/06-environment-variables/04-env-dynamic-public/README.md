---
title: $env/dynamic/public
---

Comme pour les [variables d'environnement privées](/tutorial/env-static-private), il est préférable d'utiliser des valeurs statiques si possible, mais si nécessaire, nous pouvons utiliser des valeurs dynamiques pour les variables d'environnement publiques :

```svelte
/// file: src/routes/+page.svelte
<script>
	import { +++env+++ } from '$env/+++dynamic+++/public';
</script>

<main
	style:background={+++env.+++PUBLIC_THEME_BACKGROUND}
	style:color={+++env.+++PUBLIC_THEME_FOREGROUND}
>
	{+++env.+++PUBLIC_THEME_FOREGROUND} on {+++env.+++PUBLIC_THEME_BACKGROUND}
</main>
```
