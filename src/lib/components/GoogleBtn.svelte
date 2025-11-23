<script>
    import { onMount, createEventDispatcher } from "svelte";
    import { GOOGLE_CLIENT_ID } from "$lib/config";

    const dispatch = createEventDispatcher();

    onMount(() => {
        // @ts-ignore
        if (window.google) {
            // @ts-ignore
            window.google.accounts.id.initialize({
                client_id: GOOGLE_CLIENT_ID,
                callback: handleCredentialResponse,
            });
            // @ts-ignore
            window.google.accounts.id.renderButton(
                document.getElementById("google-btn-container"),
                { theme: "outline", size: "large", width: "100%" },
            );
        }
    });

    /** @param {any} response */
    function handleCredentialResponse(response) {
        dispatch("login", response.credential);
    }
</script>

<div id="google-btn-container" style="width: 100%; margin-bottom: 1rem;"></div>
