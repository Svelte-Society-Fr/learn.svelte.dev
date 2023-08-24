---
title: Paramètres de reste
path: /how
focus: /src/routes/[path]/+page.svelte
---

Pour cibler un nombre indéfini de segments, utilisez un paramètre `[...rest]`, appelé ainsi pour sa ressemblance avec les [paramètres de reste en JavaScript](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Functions/rest_parameters).

Renommez `src/routes/[path]` en `src/routes/[...path]`. La route cible maintenant n'importe quel chemin.

> D'autres routes, plus spécifiques, seront d'abord testées, rendant les paramètres de reste efficaces en tant que routes "attrape-tout". Par exemple, si vous aviez besoin d'une page 404 personnalisée dans `/categories/...`, vous pourriez ajouter ces fichiers :
>
> ```diff
> src/routes/
> ├ categories/
> │ ├ animal/
> │ ├ mineral/
> │ ├ vegetable/
> +│ ├ [...catchall]/
> +│ │ ├ +error.svelte
> +│ │ └ +page.server.js
> ```
>
> dans le fichier `+page.server.js`, ajoutez `throw error(404)` dans la fonction `load`.

Les paramètres de reste n'ont _pas_ besoin d'être définis en dernier — une route comme `/items/[...path]/edit` ou `/items/[...path].json` est totalement valide.
