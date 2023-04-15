<script lang="ts">
	import { get } from 'svelte/store';
	import { cartItems, addCart, removeCart } from './store/cart';
	export let product: Product = { id: '', name: '', price: 0 };

	let cart = get(cartItems);
	let cartItemIndex = cart.findIndex((item) => {
		return item.id === product.id;
	});
	let cartProduct = cart[cartItemIndex];

	cartItems.subscribe((newValue) => {
		cart = newValue;
		cartItemIndex = cart.findIndex((item) => {
			return item.id === product.id;
		});
		cartProduct = cart[cartItemIndex];
	});
</script>

<div class="card">
	<header class="card-header">
		<h2>{product.name}</h2>
		{#if cartProduct !== undefined}
			<div class="card-body">
				Quantity: <strong>{cartProduct.quantity}</strong>
			</div>
		{/if}
		<div class="card-body">
			Price ${product.price}
		</div>
		<footer class="card-footer">
			<button class="variant-glass-primary rounded p-2" on:click={() => addCart(product.id)}
				>Add</button
			>
			<button class="variant-glass-error rounded p-2" on:click={() => removeCart(product.id)}
				>Remove</button
			>
		</footer>
	</header>
</div>
