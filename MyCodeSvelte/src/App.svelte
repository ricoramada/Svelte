<script>
	import Thing from './things.svelte';
	let user = {loggedIn: false};
	let x = 7;
	let motor = [
		{id: 'H846Fdga3Po', nama: 'Hayabusa'},
		{id: 'zT_ohb9XGS4', nama: 'Ninja'},
		{id: '30axCVLkPyg', nama: 'RX-King'},
	];
	let things = [
		{ id: 1, color: '#0d0887' },
		{ id: 2, color: '#6a00a8' },
		{ id: 3, color: '#b12a90' },
		{ id: 4, color: '#e16462' },
		{ id: 5, color: '#fca636' }
	];
	let promise = getRandomNumber();
	let posisi = {x: 0, y: 0};

	function mousemove(event) {
		posisi.x = event.clientX;
		posisi.y = event.clientY;
	}

	async function getRandomNumber() {
		const res = await fetch(`random-number`);
		const text = await res.text();

		if (res.ok) {
			return text;
		} else {
			throw new Error(text);
		}
	}

	function hitung() {
		promise = getRandomNumber();
	}
	function toggle(){
		user.loggedIn = !user.loggedIn;
	}
	function handleClick() {
		things = things.slice(1);
	}
</script>

<main>
	{#if x > 10}
		<p>{x} lebih dari 10</p>
	{:else}
		{#if 5 > x}
			<p>{x} kurang dari 5</p>
		{:else}
			<p>{x} diantara 5 dan 10</p>
		{/if}
	{/if}
	{#if user.loggedIn}
		<button on:click={toggle}>
			Logout
		</button>
	{:else}
		<button on:click={toggle}>
			Login
		</button>
	{/if}
	{#each motor as {id, nama}, i}
		<li><a target="_blank" href="https://www.youtube.com/watch?v={id}">
		{i + 1}: {nama}
		</a></li>
	{/each}
	<button on:click={handleClick}>
		Hapus
	</button>
	{#each things as thing (thing.id)}
	<Thing current={thing.color}/>
	{/each}

	<button on:click={hitung}>
		Random Nomer
	</button>

	{#await promise}
		<p>Mohon Tunggu...</p>
	{:then number} 
		<p>The number is {number}</p>
	{:catch error}
		<p style="color:red">{error.massage}</p>
	{/await}
	<div on:mousemove={mousemove}>
		Posisi Mouse {posisi.x} x {posisi.y}
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>