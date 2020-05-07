<script>
  import { onMount } from "svelte";
  import hobbyStore from "./hobbie-store.js";

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
      // hobbies = Object.values(data);
      hobbyStore.setHobbies(Object.values(data));
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

  const addHobby = () => {
    // console.log({ hobbieInput });
    // hobbies = [...hobbies, hobbieInput.value];
    hobbyStore.addHobby(hobbieInput.value);

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
        // ..
        alert("Saved data!");
        // res.json() => promise with a object that contains the id
      })
      .catch(error => {
        isLoading = false;
        console.log(`Error: ${error}`);
      });
  };
</script>

<label for="hobbie">Hobbie</label>
<input type="text" id="hobbie" bind:this={hobbieInput} />
<button on:click={addHobby}>Add Hobbie</button>

{#if isLoading}
  <p>Loading...</p>
{:else}
  <ul>
    {#each $hobbyStore as hobby}
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
