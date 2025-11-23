<script>
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    onMount(() => {
        goto("/login");
    });
</script>

<div class="loading">Loading...</div>

<style>
    .loading {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        color: var(--color-text-dim);
    }
</style>
