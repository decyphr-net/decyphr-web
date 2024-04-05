<script>
	/** @type {string} */
	let text_to_be_translated = '';
	/** @type {string} */
	let source_language = 'en-ie';
	/** @type {string} */
	let target_language = 'pt-br';
	/** @type {string} */
	let translator = 'deepl';
	/** @type {string} */
	let translatedText = 'Translated text will appear here';
	/** @type {string} */
	let processor = 'google';

	let processedText = '';

	const translateEndpoint = 'http://127.0.0.1:8000/translate/';
	const nlpEndpoint = 'http://127.0.0.1:8000/nlp/'

	let handleSubmit = () => {
		let data = {
			"text_to_be_translated": text_to_be_translated,
			"source_language_code": source_language,
			"target_language_code": target_language,
			"translator": translator
		}

		fetch(
			translateEndpoint, 
			{
				method: 'POST', 
				body: JSON.stringify(data),
				headers: new Headers({'content-type': 'application/json'})
			}
		).then(response => response.json()).then(data => {
			translatedText = data.translated_text
		})
  }

	let handleNlp = () => {
		let data = {
			"text_to_be_processed": translatedText,
			"language_code": source_language,
			"processor": processor
		}

		fetch(
			nlpEndpoint, 
			{
				method: 'POST', 
				body: JSON.stringify(data),
				headers: new Headers({'content-type': 'application/json'})
			}
		).then(response => response.json()).then(data => {
			console.log(data)
			processedText = data
		})
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section class="parent">
	<div class="div1"> </div>

	<div class="div2">
		<h1>Welcome to Decyphr</h1>
	</div>
	<div class="div3"></div>
	<div class="div4"></div>
	<div class="div5">
		<form on:submit|preventDefault={handleSubmit}>
			<textarea 
				class="form-control" 
				placeholder="Text to translate" 
				bind:value={text_to_be_translated} />
			<select name="source_language" bind:value={source_language}>
				<option value="en-ie">English (Ireland)</option>
				<option value="pt-br">Portuguese (Brazil)</option>
			</select>
			<select name="target_language" bind:value={target_language}>
				<option value="en-ie">English (Ireland)</option>
				<option value="pt-br">Portuguese (Brazil)</option>
			</select>
			<select name="translator" bind:value={translator}>
				<option value="deepl">Deepl</option>
				<option value="amazon">Amazon</option>
				<option value="google">Google</option>
			</select>
			{#if text_to_be_translated === ''}
				<button class="btn btn-primary" type="submit" disabled>Submit</button>
			{:else}
				<button class="btn btn-primary" type="submit">Submit</button>
			{/if}
		</form>
	</div>
	<div class="div6">
		<form on:submit|preventDefault={handleNlp}>
			{#if translatedText === 'Translated text will appear here'}
			<textarea
				readonly
				bind:value={translatedText}
				disabled />
			{:else}
			<textarea
				readonly
				bind:value={translatedText} />
			{/if}
			
			{#if translatedText === 'Translated text will appear here'}
				<select name="processor" bind:value={processor} disabled>
					<option value="amazon">Amazon</option>
					<option value="google">Google</option>
				</select>
			{:else}
				<select name="processor" bind:value={processor}>
					<option value="amazon">Amazon</option>
					<option value="google">Google</option>
				</select>
			{/if}
			{#if translatedText === 'Translated text will appear here'}
				<button class="btn btn-primary" type="submit" disabled>Submit</button>
			{:else}
				<button class="btn btn-primary" type="submit">Submit</button>
			{/if}
		</form>
		{#each processedText as processedItem, index}
			<small class="{processedItem.pos_tag.toLowerCase()}">{processedItem.text}</small>&nbsp;
		{/each}
	</div>
	<div class="div7"> </div>
</section>
