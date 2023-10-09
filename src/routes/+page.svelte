<script>
    import { Html5Qrcode } from 'html5-qrcode'
    import { onMount } from 'svelte'

    let scanning = false

    let html5Qrcode

    let codigo = "";

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
        //alert(`Code matched = ${decodedText}`)
        codigo = decodedText
        console.log(decodedResult)
        stop

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
        width: 100%;
        min-height: 300px;
        background-color: black;
    }
</style>

<main>
    <h1>El FINO</h1>
    <reader id="reader"/>
    <h3>
        {codigo}
    </h3>

    <h2>
        El precio es:
    </h2>

    <p>Cuaderno</p>

    <h1  style="color:red">{Math.floor((Math.random() * (20000 - 500 + 1)) + 500)}</h1>
    {#if scanning}
        <button on:click={stop}>Dejar de leer</button>
    {:else}
        <button on:click={start}>Leer</button>
    {/if}
</main>


