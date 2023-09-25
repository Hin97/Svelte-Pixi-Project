<script>
    import "./styles.css";
    import Snake from "./Snake.svelte";
    import { onMount } from "svelte";

    // Use webfontloader to load external fonts and render screen only if the font is loaded
    let fontLoaded = false;
    onMount(async () => {
        const WebFont = await import("webfontloader");
        WebFont.load({
            google: {
                families: ["Pacifico"],
            },
        });
        fontLoaded = true;
    });
</script>

<svelte:head>
    <title>Snake</title>
    <meta
        name="Svelte-Pixi application"
        content="Svelte-Pixi application"
    />
</svelte:head>

{#if fontLoaded}
    <Snake />
{/if}

<style>
</style>
