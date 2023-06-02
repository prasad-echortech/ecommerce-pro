<script>
	import { goto } from '$app/navigation';
	import Navprod from '../navproducts/+page.svelte';
	function ProAdd() {
		if (localStorage.getItem("currentUser") !== null) {
			goto('/add');
		}
	}
	function ProUp() {
		if (localStorage.getItem("currentUser") !== null) {
			goto('/delete');
		}
	}
	function ProDel() {
		if (localStorage.getItem("currentUser") !== null) {
			goto('/update');
		}
	}
</script>

<Navprod />
<div class="text-center mt-5">
	<h1 class="mb-5">Perform Product CRUD operations Here!</h1>
	<button type="button" class="btn btn-primary" on:click={ProAdd}>Create</button>
	<button type="button" class="btn btn-danger" on:click={ProUp}>Delete</button>
	<button type="button" class="btn btn-success" on:click={ProDel}>Update</button>
</div>
