---
title: Ignorer des layouts
---

D'habitude, une page hérite de tous ses <span class="vo">[layouts](SVELTE_SITE_URL/docs/web#layout)</span> parents, ce qui implique que la route `src/routes/a/b/c/+page.svelte` hérite de quatre <span class="vo">[layouts](SVELTE_SITE_URL/docs/web#layout)</span> :

- `src/routes/+layout.svelte`
- `src/routes/a/+layout.svelte`
- `src/routes/a/b/+layout.svelte`
- `src/routes/a/b/c/+layout.svelte`

Occasionnellement, c'est utile d'ignorer la hiérarchie de <span class="vo">[layouts](SVELTE_SITE_URL/docs/web#layout)</span> courante. Nous pouvons faire cela en ajoutant le symbole `@` suivi du nom du segment parent servant de nouvelle "base". Par exemple, prenons le fichier de page de la route `src/routes/a/b/c` ; si on le nomme :
- `+page.svelte`, il utilise les layouts des répertoires `a`, `a/b`, et `a/b/c`
- `+page@b.svelte`, il utilise les layouts des répertoires `a` et `a/b`, mais *pas* de `a/b/c`
- `+page@a.svelte`, il utilise le layout du répertoire `a`, mais *pas* de `a/b/c`, *ni* de `a/b`

Réinitialisons le <span class="vo">[layout](SVELTE_SITE_URL/docs/web#layout)</span> jusqu'au layout racine, en renommant le fichier `+page@.svelte`.

> Le <span class="vo">[layout](SVELTE_SITE_URL/docs/web#layout)</span> racine s'applique à toutes les pages de votre application, vous ne pouvez pas y échapper.
