<script lang="ts">
	import ProductCard from '$lib/productCard.svelte';
	import { get } from 'svelte/store';
	import { cartItems } from '$lib/store/cart';

	const products: Product[] = [
		{
			id: 'price_1MxCKTAh4JJQMbCgMyhK65QE',
			name: 'Coffee',
			price: 5
		},
		{
			id: 'price_1MxCL3Ah4JJQMbCgtDv5igPB',
			name: 'Sunglasses',
			price: 10
		},
		{
			id: 'price_1MxCLPAh4JJQMbCgaitxgEjh',
			name: 'Water Bottle',
			price: 15
		}
	];

	async function checkout() {
		await fetch('api/stripeCheckout', {
			// http://localhost:5173/api/stripeCheckout
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({ items: get(cartItems) })
		})
			.then((data) => {
				return data.json();
			})
			.then((data) => {
				window.location.replace(data.url);
			});
	}
</script>

<div class=" container mx-auto flex h-full items-center justify-center">
	<div class="grid grid-cols-3 gap-4">
		<div class="col-span-3">
			<h1>SvelteKit 1.0 Store</h1>
		</div>
		{#each products as product}
			<ProductCard {product} />
		{/each}
		<div class="col-span-3">
			<button class="btn variant-filled-primary" on:click={() => checkout()}
				>Checkout with Stripe API</button
			>
		</div>
	</div>
</div>
