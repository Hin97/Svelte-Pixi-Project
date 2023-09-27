<script>
    import svelte from "$lib/images/svelte-logo.svg";
    import github from "$lib/images/github.svg";
    import pixi from "$lib/images/pixijs-logo-mark-light.svg";
</script>

<div id="sectionContainer">
    <h1>Credits</h1>
    <div id="logoContainer">
        <a href="https://kit.svelte.dev">
            <img
                src={svelte}
                alt="SvelteKit"
            />
        </a>
        <a href="https://pixijs.com/">
            <img
                src={pixi}
                alt="pixiJS"
            />
        </a>
        <a href="https://github.com/Hin97?tab=repositories">
            <img
                src={github}
                alt="Github"
            />
        </a>
    </div>
</div>

<style>
    #sectionContainer {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    img {
        width: 60px;
        height: 60px;
    }
    #logoContainer {
        display: flex;
        width: auto;
        flex-direction: row;
        padding: 30px;
    }
    h1 {
        font-family: "Pacifico";
    }
</style>
