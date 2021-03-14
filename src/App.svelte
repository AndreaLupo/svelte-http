<script>


	let hobbies = [];
	let hobbyInput;

	function addHobby() {
		hobbies = [...hobbies, hobbyInput.value];

		fetch('https://svelte-course-a2f4f-default-rtdb.firebaseio.com/hobbies.json', 
			{
				method: 'POST',
				body: JSON.stringify(hobbies),
				headers: {
					'Content-Type': 'application/json'
				}
			}
		).then(res => {
			if(!res.ok) {
				throw new Error("Faild!");
			}
		}).catch(err => {
			console.log(err);
		});
	}
</script>

<label for="hobby">Hobby</label>
<input type="text" id="hobby" bind:this={hobbyInput}>
<button on:click={addHobby}>Add hobby</button>

<ul>
	{#each hobbies as hobby}
		<li>{hobby}</li>
	{/each}
</ul>