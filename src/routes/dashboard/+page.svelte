<script>
    import GlassCard from "$lib/components/GlassCard.svelte";
    import { goto } from "$app/navigation";
    import { onMount } from "svelte";

    /** @type {any} */
    let user = null;

    onMount(() => {
        const userData = localStorage.getItem("user");
        if (userData) {
            user = JSON.parse(userData);
        } else {
            goto("/login");
        }
    });

    function handleLogout() {
        localStorage.removeItem("user");
        goto("/login");
    }

    const contacts = [
        { name: "Kisan Call Center", number: "1800-180-1551", type: "Support" },
        {
            name: "Local Mandi Officer",
            number: "+91 98765 43210",
            type: "Government",
        },
        {
            name: "Seed Supplier (Ravi)",
            number: "+91 99887 76655",
            type: "Supplier",
        },
        {
            name: "Fertilizer Depot",
            number: "+91 88776 65544",
            type: "Supplier",
        },
    ];
</script>

<div class="dashboard-container">
    <header>
        <div class="logo">AgriConnect</div>
        <div class="user-info">
            {#if user}
                <span>Welcome, {user.username || "Farmer"}</span>
            {/if}
            <button class="logout-btn" on:click={handleLogout}>Log Out</button>
        </div>
    </header>

    <main>
        <h1 class="section-title">Important Contacts</h1>

        <div class="contacts-grid">
            {#each contacts as contact}
                <GlassCard>
                    <div class="contact-card">
                        <div class="contact-icon">📞</div>
                        <div class="contact-details">
                            <h3>{contact.name}</h3>
                            <p class="number">{contact.number}</p>
                            <span class="type-badge">{contact.type}</span>
                        </div>
                    </div>
                </GlassCard>
            {/each}
        </div>
    </main>
</div>

<style>
    .dashboard-container {
        min-height: 100vh;
    }

    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 1.5rem 2rem;
        background: rgba(0, 0, 0, 0.2);
        backdrop-filter: blur(10px);
        border-bottom: 1px solid var(--glass-border);
    }

    .logo {
        font-size: 1.5rem;
        font-weight: 700;
        color: var(--color-accent);
    }

    .user-info {
        display: flex;
        align-items: center;
        gap: 1.5rem;
    }

    .logout-btn {
        padding: 8px 16px;
        background: rgba(255, 255, 255, 0.1);
        border: 1px solid var(--glass-border);
        color: var(--color-text-light);
        border-radius: 6px;
        cursor: pointer;
        transition: all 0.3s ease;
    }

    .logout-btn:hover {
        background: rgba(239, 68, 68, 0.2); /* Red tint */
        border-color: rgba(239, 68, 68, 0.5);
    }

    main {
        padding: 2rem;
        max-width: 1200px;
        margin: 0 auto;
    }

    .section-title {
        font-size: 2rem;
        margin-bottom: 2rem;
        color: var(--color-text-light);
    }

    .contacts-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
        gap: 1.5rem;
    }

    .contact-card {
        display: flex;
        align-items: center;
        gap: 1rem;
    }

    .contact-icon {
        font-size: 2rem;
        background: rgba(255, 255, 255, 0.1);
        padding: 1rem;
        border-radius: 50%;
    }

    .contact-details h3 {
        margin-bottom: 0.25rem;
        font-size: 1.1rem;
    }

    .number {
        color: var(--color-accent);
        font-weight: 600;
        margin-bottom: 0.5rem;
    }

    .type-badge {
        font-size: 0.8rem;
        padding: 4px 8px;
        background: rgba(255, 255, 255, 0.1);
        border-radius: 12px;
        color: var(--color-text-dim);
    }
</style>
