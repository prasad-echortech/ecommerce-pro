<!-- <script>
	import Nav from './nav/+page.svelte';
	import { goto } from '$app/navigation';
	import { browser } from '$app/environment';

	let userdata;
	let userdata1;
	if (browser) {
		userdata = localStorage.getItem('currentUser');
		userdata1 = JSON.parse(userdata);
		localStorage.removeItem('currentUser');
	}

	(async () => {
		 await fetch('http://localhost:3000/Products')
			.then((res) => res.json())
			.then((res) => {
				console.log(res);
				myCardArr = [...res];
			});
	})();

	let myCardArr = [];

	function info(params) {
		localStorage.setItem('UsrInfoID', JSON.stringify(params));
		goto('/sinfo');
	}
</script>

<Nav />

<div class="text-center">
	Register Here<a href="/register"> Click?</a> Already Registered
	<a href="/login"><button class="btn bg-success mt-2 fs-5 text-white fw-bold">Login</button></a>
</div>

<div
	class="container-fluid d-flex flex-row flex-wrap text-center justify-content-center mt-5"
	id="wholecard"
>
	{#each myCardArr as cards}
		<div class="card ms-3 mb-2" style="width: 15rem;">
			<div class="card-body border border-success">
				<h5 class="card-title">Title : {cards.name}</h5>
				<h5 class="card-title">Product-Id : {cards.id}</h5>
				<p class="card-text">
					Lorem ipsum, dolor sit amet consectetur adipisicing elit. Expedita dolorem hic aut
					architecto perspiciatis magni adipisci quo nihil ea, unde facilis molestiae facere sint
					nam labore, quisquam minima excepturi exercitationem!
				</p>
				<h5 class="card-title">Price : {cards.price}/-</h5>
				<div>
					<a href="/register" class="btn btn-primary mb-3">Add to Cart🛒</a>
				</div>
				<a href="#">
					<button class="btn btn-secondary" on:click={info(cards.id)}>Info</button>
				</a>
			</div>
		</div>
	{/each}
</div>
 -->
<script>
	import { browser } from '$app/environment';
	import Nav from './nav/+page.svelte';
	import { onMount, afterUpdate } from 'svelte';
	import { writable, derived } from 'svelte/store';
	import { goto } from '$app/navigation';

	const pageSize = 5; // Number of items per page
	const dataset = writable([]); // Your dataset goes here

	// Calculate the number of pages
	const totalPages = derived(dataset, ($dataset) => Math.ceil($dataset.length / pageSize));

	// Current page store
	const currentPage = writable(1);

	// Computed property to slice the dataset based on the current page
	const paginatedData = derived([dataset, currentPage], ([$dataset, $currentPage]) => {
		const start = ($currentPage - 1) * pageSize;
		const end = start + pageSize;
		return $dataset.slice(start, end);
	});

	// Fetch or initialize your dataset
	onMount(async () => {
		const response = await fetch('http://localhost:3000/Products');
		const data = await response.json();
		dataset.set(data);
	});

	let myCardArr = [];

	// Update myCardArr whenever paginatedData changes
	afterUpdate(() => {
		myCardArr = $paginatedData;
	});

	function info(params) {
		localStorage.setItem('UsrInfoID', JSON.stringify(params));
		goto('/sinfo');
	}

	// Handle page navigation
	function goToPage(page) {
		currentPage.set(page);
	}
	
</script>

<Nav />

<div class="text-center">
	Register Here<a href="/register"> Click?</a> Already Registered
	<a href="/login"><button class="btn bg-success mt-2 fs-5 text-white fw-bold">Login</button></a>
</div>

<div
	class="container-fluid d-flex flex-row flex-wrap text-center justify-content-center mt-5"
	id="wholecard"
>
	{#each myCardArr as cards}
		<div class="card ms-3 mb-2" style="width: 15rem;">
			<div class="card-body border border-success">
				<h5 class="card-title">Title: {cards.name}</h5>
				<h5 class="card-title">Product-Id: {cards.id}</h5>
				<p class="card-text">
					Lorem ipsum, dolor sit amet consectetur adipisicing elit. Expedita dolorem hic aut
					architecto perspiciatis magni adipisci quo nihil ea, unde facilis molestiae facere sint
					nam labore, quisquam minima excepturi exercitationem!
				</p>
				<h5 class="card-title">Price: {cards.price}/-</h5>
				<div>
					<a href="/register" class="btn btn-primary mb-3">Add to Cart🛒</a>
				</div>
				<a href="#">
					<button class="btn btn-secondary" on:click={() => info(cards.id)}>Info</button>
				</a>
			</div>
		</div>
	{/each}
</div>

<!-- Display pagination controls -->
<div class="text-center mt-4 mb-3">
	{#each Array.from({ length: $totalPages }) as _, i}
		<button class="btn btn-dark m-1 shadow-lg" on:click={() => goToPage(i + 1)}> {i + 1} </button>
	{/each}
</div>
