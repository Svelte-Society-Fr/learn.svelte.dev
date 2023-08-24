<script>
	import { page } from '$app/stores';
</script>

<nav
	class:has-color={!!$page.data.color}
	style:background={$page.data.color ?? 'var(--bg-2)'}
>
	<a href="/">accueil</a>
	<a href="/red">rouge</a>
	<a href="/green">vert</a>
	<a href="/blue">bleu</a>
</nav>

<slot />

<style>
	nav.has-color,
	nav.has-color a {
		color: white;
	}
</style>
