<script>
	import sound from '$lib/Beep-sound.mp3';
	import { Html5Qrcode } from 'html5-qrcode';
	import { onMount } from 'svelte';

	let scanning = false;

	// @ts-ignore
	let audio;

	// @ts-ignore
	let html5Qrcode;

	let codigo = '';

	onMount(init);

	function init() {
		html5Qrcode = new Html5Qrcode('reader');
	}

	function start() {
		// @ts-ignore
		codigo = '';
		if (!html5Qrcode) {
			html5Qrcode = new Html5Qrcode('reader');
		}
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
		// @ts-ignore
		await html5Qrcode.stop();
		scanning = false;
	}

	// @ts-ignore
	async function onScanSuccess(decodedText, decodedResult) {
		//alert(`Code matched = ${decodedText}`)
		// @ts-ignore

		audio.play();
		codigo = decodedText;
		console.log(decodedResult);
		// @ts-ignore
		await html5Qrcode.stop();
		scanning = false;
	}

	// @ts-ignore
	function onScanFailure(error) {
		console.warn(`Code scan error = ${error}`);
	}

	function startTimer() {
		audio.play();
	}
</script>

<main>
	<h1>El FINO</h1>

	{#if scanning}
		<button on:click={stop}>Detener</button>
	{:else}
		<button on:click={start}>Ver precio</button>
		{#key codigo}
			{#if codigo !== ''}
				<p style="color:blue">$ {Math.floor(Math.random() * (20000 - 500 + 1) + 500)}</p>
				<p>
					{codigo}
				</p>
			{/if}
		{/key}
	{/if}

	<reader class={scanning ? 'a' : 'b'} id="reader" />

	<!--button on:click={startTimer}>
		start 2s timer
	</button-->
	<audio src={sound} bind:this={audio} />
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

	.a {
		visibility: visible;
	}

	.b {
		visibility: hidden;
	}
</style>
