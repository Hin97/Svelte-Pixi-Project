<script>
    import "./styles.css";
    import Snake from "./Snake.svelte";
    import { onMount } from "svelte";
    import Instruction from "./Instruction.svelte";
    import Credits from "./Credits.svelte";

    // Use webfontloader to load external fonts and render screen only if the font is loaded
    let fontLoaded = false;
    onMount(async () => {
        const WebFont = await import("webfontloader");
        WebFont.load({
            google: {
                families: ["VT323", "Pacifico"],
            },
            active: () => {
                fontLoaded = true;
            },
        });
    });
</script>

<svelte:head>
    <title>Snake Game</title>
    <meta
        name="Svelte-Pixi application"
        content="Svelte-Pixi application"
    />
</svelte:head>

{#if fontLoaded}
    <div id="main-container">
        <div id="section"><Instruction /></div>
        <div id="section"><Snake /></div>
        <div id="section"><Credits /></div>
    </div>
{/if}

<style>
    #main-container {
        display: flex;
        width: 100vw;
        height: 100vh;
        flex-direction: row;
        flex-wrap: wrap;
        align-items: center;
    }
    #section {
        flex: 1;
        display: flex;
        height: 460px;
        justify-content: center;
    }
</style>
