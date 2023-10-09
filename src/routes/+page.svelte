<script>
    import { Html5Qrcode } from 'html5-qrcode'
    import { onMount } from 'svelte'

    let scanning = false

    let html5Qrcode

    let codigo = "nada";

    onMount(init)

    function init() {
        html5Qrcode = new Html5Qrcode('reader')
    }

    function start() {
        html5Qrcode.start(
            { facingMode: 'environment' },
            {
                fps: 10,
                qrbox: { width: 250, height: 250 },
            },
            onScanSuccess,
            onScanFailure
        )
        scanning = true
    }

    async function stop() {
        await html5Qrcode.stop()
        scanning = false
    }

    function onScanSuccess(decodedText, decodedResult) {
        alert(`Code matched = ${decodedText}`)
        codigo = decodedText
        console.log(decodedResult)
    }

    function onScanFailure(error) {
        console.warn(`Code scan error = ${error}`)
    }
</script>

<style>
    main {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 20px;
    }
    reader {
        width: 80%;
        min-height: 500px;
        background-color: black;
    }
</style>

<main>
    <h1>Andres, leo codigo de barras</h1>
    <reader id="reader"/>
    <h1>
        {codigo}
    </h1>
    {#if scanning}
        <button on:click={stop}>Dejar de leer</button>
    {:else}
        <button on:click={start}>Leer</button>
    {/if}
</main>


