<script>
	import PackageInfo from './PackageInfo.svelte';

	const pkg = {
		name: 'svelte',
		speed: 'incroyablement',
		version: 4,
		website: 'https://svelte.dev'
	};
</script>

<PackageInfo
	name={pkg.name}
	speed={pkg.speed}
	website={pkg.website}
/>
