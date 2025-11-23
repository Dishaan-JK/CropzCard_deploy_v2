<script>
    import { page } from "$app/stores";

    const links = [
        { href: "/dashboard", label: "Dashboard", icon: "📊" },
        { href: "/orders", label: "Orders", icon: "📝" },
        { href: "/contacts", label: "Contacts", icon: "👥" },
        { href: "/settings", label: "Settings", icon: "⚙️" },
    ];
</script>

<aside class="sidebar">
    <nav>
        {#each links as link}
            <a
                href={link.href}
                class="nav-link {$page.url.pathname === link.href
                    ? 'active'
                    : ''}"
            >
                <span class="icon">{link.icon}</span>
                <span class="label">{link.label}</span>
            </a>
        {/each}
    </nav>
</aside>

<style>
    .sidebar {
        width: 250px;
        background: var(--card-bg);
        border-right: 1px solid var(--card-border);
        height: calc(100vh - 70px); /* Adjust based on header height */
        padding: 1.5rem;
        position: sticky;
        top: 70px;
        display: flex;
        flex-direction: column;
    }

    .nav-link {
        display: flex;
        align-items: center;
        gap: 1rem;
        padding: 12px 16px;
        color: var(--text-secondary);
        border-radius: 12px;
        margin-bottom: 0.5rem;
        transition: all 0.3s ease;
        font-weight: 500;
    }

    .nav-link:hover {
        background: rgba(0, 0, 0, 0.03);
        color: var(--color-primary);
    }

    .nav-link.active {
        background: var(--color-primary);
        color: white;
        box-shadow: 0 4px 12px rgba(5, 150, 105, 0.2);
    }

    .icon {
        font-size: 1.2rem;
    }

    @media (max-width: 768px) {
        .sidebar {
            display: none; /* Mobile menu logic would go here */
        }
    }
</style>
