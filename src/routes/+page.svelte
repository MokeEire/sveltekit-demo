<script>
	import Counter from './Counter.svelte';
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';
	import { onMount } from 'svelte';
	//import ReactiveDeclarations from './ReactiveDeclarations.svelte';

	let m = { x: 0, y: 0 };

	let divPad = 5;

	function increasePad() {
		divPad = divPad + 5;
	}

	function handleMousemove(event) {
		m.x = event.clientX;
		m.y = event.clientY;
	}

	let quantity = 0;
	function addToCart() {
		inventory.push(quantity);
		inventory = inventory;
		quantity = ++quantity;
	}
	let inventory = [];
	let user = { name: 'Steph' };
	$: remaining = 10 - quantity;
	$: console.log(`the quantity is ${quantity}`);
	$: {
		console.log(`the quantity is ${quantity}`);
		alert(`There are ${remaining} products remaining`);
	}
	$: if (quantity >= 10) {
		alert(`You have too many items in your cart!`);
		quantity = 9;
	}

	onMount(async () => {
		const res = await fetch(`/tutorial/api/album`);
		photos = await res.json();
	});
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<img src={welcome_fallback} alt="Welcome" />
			</picture>
		</span>

		to your new<br />SvelteKit app
	</h1>

	<h2>
		try editing <strong>src/routes/+page.svelte</strong>
	</h2>

	<Counter />

	<div>Your shopping cart has {quantity} items.</div>
	<button on:click={addToCart}>Add To Cart</button>

	<div on:mousemove={handleMousemove} style="width:100%;height:100%;padding:{divPad}px">
		This is the mouse move listener from the <a href="https://svelte.dev/tutorial/dom-events"
			>Svelte tutorial</a
		>. Your mouse position is {m.x} x {m.y}
	</div>
	<button on:click={increasePad}>Increase padding by 5px</button>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
