<script>
	import sound from '$lib/Beep-sound.mp3'
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
		audio.play()
		/*setTimeout(() => {
			// @ts-ignore
			audio.play()
		},1000)*/	
	}
</script>

<main>
	<h1>El FINO</h1>

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
        <h1 style="color:blue">$ {Math.floor(Math.random() * (20000 - 500 + 1) + 500)}</h1>
	{/key}
	<!--button on:click={startTimer}>
		start 2s timer
	</button-->
	<audio src={sound} bind:this={audio}></audio>

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
