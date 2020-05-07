<script>
  import { onMount } from "svelte";

  let hobbies = [];
  let hobbieInput;
  let isLoading = false;

  isLoading = true;
  let getHobbies = fetch("https://svelte-rest-api.firebaseio.com/hobbies.json")
    .then(result => {
      if (!result.ok) {
        throw new Error("Failed!");
      }
      return result.json();
    })
    .then(data => {
      isLoading = false;
      console.log(data);
      hobbies = Object.values(data);
      let keys = Object.keys(data);
      console.log(keys);

      for (let key in data) {
        console.log(key, data[key]);
      }
      // return hobbies;
    })
    .catch(error => {
      isLoading = false;
      console.log(`Error: ${error}`);
    });

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

<!-- {#await getHobbies}
  <p>Loading...</p>
{:then hobbyData}
  <ul>
    {#each hobbyData as hobby}
      <li>{hobby}</li>
    {/each}
  </ul>
{:catch error}
  <p>{error.message}</p>
{/await} -->
