<script>
	import {onMount} from 'svelte';

	let hobbies = [];
	let hobbyInput;
	let isLoading = false;

	onMount( () => {
		isLoading = true;
		fetch('https://svelte-course-a2f4f-default-rtdb.firebaseio.com/hobbies.json')
		.then(res => {
			if(!res.ok) {
				throw new Error("Faild!");
			}
			// convert from json to JS data
			return res.json();
		})
		.then( data => {
			isLoading = false;
			console.log(data);
			// data
			hobbies = Object.values(data);
			
			// keys
			let keys = Object.keys(data);
			console.log(keys);

			for(let key in data) {
				console.log(key, data[key]);
			}
		})
		.catch();
	});
	

	function addHobby() {
		hobbies = [...hobbies, hobbyInput.value];

		isLoading = true;

		fetch('https://svelte-course-a2f4f-default-rtdb.firebaseio.com/hobbies.json', 
			{
				method: 'POST',
				body: JSON.stringify(hobbyInput.value),
				headers: {
					'Content-Type': 'application/json'
				}
			}
		).then(res => {
			isLoading = false;
			if(!res.ok) {
				throw new Error("Faild!");
			}
			alert('Saved data!');
		}).catch(err => {
			isLoading = false;
			console.log(err);
		});
	}
</script>

<label for="hobby">Hobby</label>
<input type="text" id="hobby" bind:this={hobbyInput}>
<button on:click={addHobby}>Add hobby</button>



{#if isLoading}
	<p>Loading...</p>
{:else}
	<ul>
		{#each hobbies as hobby}
			<li>{hobby}</li>
		{/each}
	</ul>
{/if}