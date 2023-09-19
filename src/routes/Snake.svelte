<script>
    // @ts-nocheck

    // YourSvelteComponent.svelte
    import { onMount } from "svelte";
    import * as PIXI from "pixi.js";

    let app; // PixiJS Application instance

    onMount(() => {
        // Create a PixiJS Application with a WebGL renderer
        app = new PIXI.Application({
            width: 605,
            height: 480,
            background: "#1099bb",
        });

        // Append the PixiJS Application's view (canvas) to your Svelte component
        document.getElementById("pixi-container").appendChild(app.view);

        // Your PixiJS code (e.g., creating sprites, graphics, etc.) goes here
    });
</script>

<div id="pixi-container" />

<style>
</style>
