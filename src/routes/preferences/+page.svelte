<script>
  import { onMount } from "svelte";

  let languages = [];
  let preferences = { };
  let translator = 'google';
  let processor = 'google';
  let source_language = '';
  let target_language = '';

  const preferencesEndpoint = `${import.meta.env.VITE_BASE_URL}preferences/`

  onMount(() => {
    const languagesEndpoint = `${import.meta.env.VITE_BASE_URL}languages/`
    
    fetch(
			languagesEndpoint, 
			{
				method: 'GET', 
				headers: new Headers({'content-type': 'application/json'})
			}
		).then(response => response.json()).then(data => {
			languages = data
		})

    fetch(
      preferencesEndpoint,
      {
				method: 'GET', 
				headers: new Headers({'content-type': 'application/json'})
			}
    ).then(response => response.json()).then(data => {
      preferences = data;

      translator = preferences.translator;
      processor = preferences.processor;
      source_language = preferences.source_lang;
      target_language = preferences.target_lang;
    })
  })

  let updatePreferences = () => {
    let data = {
      "translator": translator,
      "processor": processor,
      "source_lang": source_language,
      "target_lang": target_language
    }

    fetch(
      `${preferencesEndpoint + preferences.id}/`,
      {
        method: 'PATCH',
        body: JSON.stringify(data),
        headers: new Headers({'content-type': 'application/json'})
      }
    ).then(response => response.json()).then(data => {
      preferences = data
    });
  }

</script>

<section class="parent">
  <div class="heading-spacer-1"></div>
  <div class="heading-content">
    <h1>Preferences</h1>
  </div>
  <div class="heading-spacer-2"></div>

  <div class="body-spacer-1"></div>
  <div class="body-content">
    <form on:submit|preventDefault={updatePreferences}>
      <select name="source_language" bind:value={source_language}>
        <option value="" disabled selected>Choose your native language</option>
        {#each languages as language, index}
          <option value="{language.id}">{language.name}</option>
        {/each}
			</select>
      <select name="target_language" bind:value={target_language}>
        <option value="" disabled selected>Choose the language that you are learning</option>
				{#each languages as language, index}
          <option value="{language.id}">{language.name}</option>
        {/each}
			</select>
      <select name="translator" bind:value={translator}>
				<option value="deepl">Deepl</option>
				<option value="amazon">Amazon</option>
				<option value="google">Google</option>
			</select>
      <select name="processor" bind:value={processor}>
        <option value="amazon">Amazon</option>
        <option value="google">Google</option>
      </select>
      <button class="btn btn-primary" type="submit">Submit</button>
    </form>
  </div>

  <div class="body-spacer-2"></div>
  
</section>

<style>
  .parent {
    display: grid;
    grid-template-columns: repeat(10, 1fr);
    grid-template-rows: repeat(5, 1fr);
    grid-column-gap: 0px;
    grid-row-gap: 0px;
  }

  .heading-spacer-1 { grid-area: 1 / 1 / 2 / 3; }
  .heading-content { grid-area: 1 / 3 / 2 / 9; }
  .heading-spacer-2 { grid-area: 1 / 9 / 2 / 11; }
  .body-spacer-2 { grid-area: 2 / 1 / 3 / 4; }
  .body-content { grid-area: 2 / 4 / 3 / 8; }
  .body-spacer-2 { grid-area: 2 / 8 / 3 / 11; } 

  .heading-spacer-1, .heading-content, .heading-spacer-2 {
    height: 100px;
  }
</style>