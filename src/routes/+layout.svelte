<script>
    import "../app.css";
    import AnimatedBackground from "$lib/components/AnimatedBackground.svelte";
    import Header from "$lib/components/Header.svelte";
    import Sidebar from "$lib/components/Sidebar.svelte";
    import { page } from "$app/stores";

    $: isAuthPage =
        $page.url.pathname === "/login" || $page.url.pathname === "/register";
</script>

<AnimatedBackground />

{#if !isAuthPage}
    <Header />
{/if}

<div class="app-layout">
    {#if !isAuthPage}
        <Sidebar />
    {/if}

    <main class={!isAuthPage ? "content-with-sidebar" : ""}>
        <slot />
    </main>
</div>

<style>
    .app-layout {
        display: flex;
        min-height: 100vh;
    }

    main {
        flex: 1;
        position: relative;
        z-index: 1;
        display: flex;
        flex-direction: column;
    }

    .content-with-sidebar {
        width: 100%;
        max-width: 100%;
        overflow-x: hidden;
    }
</style>
