<script>
	export let form;
</script>

<form method="POST">
	<label>
		entrez le mot de passe
		<input name="passphrase" autocomplete="off" />
	</label>
</form>

{#if form?.incorrect}
	<p class="error">mot de passe incorrect !</p>
{/if}

<style>
	.error {
		color: red;
	}
</style>
