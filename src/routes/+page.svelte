<script>
	let name = '';

	let handleSubmit = () => {
		console.log(name)
  }
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>Welcome to Decyphr</h1>

	<form on:submit|preventDefault={handleSubmit}>
		<input class="form-control" type="text" placeholder="name" bind:value={name}/>
		<button class="btn btn-primary" type="submit">Submit</button>
	</form>
</section>
