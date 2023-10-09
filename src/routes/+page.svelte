<script>
	import { Html5Qrcode } from 'html5-qrcode';
	import { onMount } from 'svelte';

	let scanning = false;

	let html5Qrcode;

	let codigo = '';

	onMount(init);

	function init() {
		html5Qrcode = new Html5Qrcode('reader');
	}

	function start() {
		html5Qrcode.start(
			{ facingMode: 'environment' },
			{
				fps: 10,
				qrbox: { width: 250, height: 250 }
			},
			onScanSuccess,
			onScanFailure
		);
		scanning = true;
	}

	async function stop() {
		await html5Qrcode.stop();
		scanning = false;
	}

	async function onScanSuccess(decodedText, decodedResult) {
		//alert(`Code matched = ${decodedText}`)
		codigo = decodedText;
		console.log(decodedResult);
		await html5Qrcode.stop();
		scanning = false;
	}

	function onScanFailure(error) {
		console.warn(`Code scan error = ${error}`);
	}
</script>

<main>
	<h1>El FINO</h1>
    {#key codigo}
	    <h1 style="color:blue">$ {Math.floor(Math.random() * (20000 - 500 + 1) + 500)}</h1>
    {/key}
	{#if scanning}
		<button on:click={stop}>Detener</button>
	{:else}
		<button on:click={start}>Ver precio</button>
	{/if}
	<reader id="reader" />
	{#key codigo}
		<h3>
			{codigo}
		</h3>
	{/key}

</main>

<style>
	main {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 20px;
	}
	reader {
		width: 100%;
		min-height: 300px;
		background-color: black;
	}
</style>
