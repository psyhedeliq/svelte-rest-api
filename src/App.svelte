<script>
  let hobbies = [];
  let hobbieInput;
  let isLoading = false;

  const addHobbie = () => {
    // console.log({ hobbieInput });
    hobbies = [...hobbies, hobbieInput.value];

    isLoading = true;
    fetch("https://svelte-rest-api.firebaseio.com/hobbies.json", {
      method: "POST",
      body: JSON.stringify(hobbieInput.value),
      headers: {
        "Content-Type": "application/json"
      }
    })
      .then(result => {
        isLoading = false;
        if (!result.ok) {
          throw new Error("Failed!");
        }
        alert("Saved data!");
      })
      .catch(error => {
        isLoading = false;
        console.log(`Error: ${error}`);
      });
  };
</script>

<label for="hobbie">Hobbie</label>
<input type="text" id="hobbie" bind:this={hobbieInput} />
<button on:click={addHobbie}>Add Hobbie</button>

{#if isLoading}
  <p>Loading...</p>
{:else}
  <ul>
    {#each hobbies as hobby}
      <li>{hobby}</li>
    {/each}
  </ul>
{/if}
