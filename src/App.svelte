<script>
  let hobbies = [];
  let hobbieInput;

  const addHobbie = () => {
    // console.log({ hobbieInput });
    hobbies = [...hobbies, hobbieInput.value];

    fetch("https://svelte-rest-api.firebaseio.com/hobbies.json", {
      method: "POST",
      body: JSON.stringify(hobbies),
      headers: {
        "Content-Type": "application/json"
      }
    })
      .then(result => {
        if (!result.ok) {
          throw new Error("Failed!");
        }
      })
      .catch(error => {
        console.log(`Error: ${error}`);
      });
  };
</script>

<label for="hobbie">Hobbie</label>
<input type="text" id="hobbie" bind:this={hobbieInput} />
<button on:click={addHobbie}>Add Hobbie</button>

<ul>
  {#each hobbies as hobby}
    <li>{hobby}</li>
  {/each}
</ul>
