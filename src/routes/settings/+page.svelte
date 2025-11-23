<script>
    import GlassCard from "$lib/components/GlassCard.svelte";
    import { theme } from "$lib/stores/theme";
    import { onMount } from "svelte";

    let user = {
        name: "",
        contact: "",
        userType: "",
    };

    onMount(() => {
        const userData = localStorage.getItem("user");
        if (userData) {
            user = JSON.parse(userData);
        }
    });

    function toggleTheme() {
        theme.update((t) => (t === "light" ? "dark" : "light"));
    }
</script>

<div class="settings-page">
    <h1 class="page-title">Settings</h1>

    <div class="settings-grid">
        <GlassCard>
            <h2>Profile</h2>
            <div class="profile-info">
                <div class="info-row">
                    <span class="label">Name:</span>
                    <span class="value">{user.name || "Not set"}</span>
                </div>
                <div class="info-row">
                    <span class="label">Contact:</span>
                    <span class="value">{user.contact || "Not set"}</span>
                </div>
                <div class="info-row">
                    <span class="label">Type:</span>
                    <span class="value badge">{user.userType || "Unknown"}</span
                    >
                </div>
            </div>
        </GlassCard>

        <GlassCard>
            <h2>Appearance</h2>
            <div class="setting-item">
                <span>Theme</span>
                <button class="theme-toggle" on:click={toggleTheme}>
                    Switch to {$theme === "light" ? "Dark" : "Light"} Mode
                </button>
            </div>
        </GlassCard>

        <GlassCard>
            <h2>App Info</h2>
            <div class="info-row">
                <span class="label">Version:</span>
                <span class="value">1.0.0 (CropzCard)</span>
            </div>
        </GlassCard>
    </div>
</div>

<style>
    .settings-page {
        padding: 2rem;
    }

    .page-title {
        font-size: 2rem;
        margin-bottom: 2rem;
        color: var(--text-main);
    }

    .settings-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
        gap: 1.5rem;
    }

    h2 {
        margin-top: 0;
        margin-bottom: 1.5rem;
        color: var(--color-primary);
        font-size: 1.2rem;
    }

    .info-row {
        display: flex;
        justify-content: space-between;
        padding: 0.5rem 0;
        border-bottom: 1px solid var(--card-border);
    }

    .info-row:last-child {
        border-bottom: none;
    }

    .label {
        color: var(--text-secondary);
    }

    .value {
        color: var(--text-main);
        font-weight: 500;
    }

    .badge {
        background: var(--color-primary);
        color: white;
        padding: 2px 8px;
        border-radius: 12px;
        font-size: 0.8rem;
    }

    .setting-item {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .theme-toggle {
        padding: 8px 16px;
        background: rgba(0, 0, 0, 0.1);
        border: 1px solid var(--card-border);
        color: var(--text-main);
        border-radius: 6px;
        cursor: pointer;
        transition: all 0.3s;
    }

    .theme-toggle:hover {
        background: var(--color-primary);
        color: white;
    }
</style>
