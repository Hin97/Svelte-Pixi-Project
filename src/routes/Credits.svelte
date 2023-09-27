<script>
    import svelte from "$lib/images/svelte-logo.svg";
    import github from "$lib/images/github.svg";
    import pixi from "$lib/images/pixijs-logo-mark-light.svg";
</script>

<div id="sectionContainer">
    <h1>Credits</h1>
    <div id="logoContainer">
        <a
            href="https://kit.svelte.dev"
            target="_blank"
        >
            <img
                src={svelte}
                alt="SvelteKit"
            />
        </a>
        <a
            href="https://pixijs.com/"
            target="_blank"
        >
            <img
                src={pixi}
                alt="pixiJS"
            />
        </a>
        <a
            href="https://github.com/Hin97?tab=repositories"
            target="_blank"
        >
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
        width: 100%;
    }
    img {
        width: 50px;
        height: 50px;
    }
    #logoContainer {
        display: flex;
        width: 50%;
        justify-content: space-between;
        flex-direction: row;
        padding: 30px;
    }
    h1 {
        font-family: "Pacifico";
    }
</style>
