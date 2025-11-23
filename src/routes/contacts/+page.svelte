<script>
    import GlassCard from "$lib/components/GlassCard.svelte";

    let activeTab = "Farmer";
    const tabs = ["Farmer", "Dealer", "Wholesaler"];

    /** @type {Record<string, any[]>} */
    const contactsData = {
        Farmer: [
            {
                name: "Ravi Kumar",
                location: "Village A",
                phone: "+91 98765 43210",
                crop: "Wheat",
            },
            {
                name: "Suresh Singh",
                location: "Village B",
                phone: "+91 87654 32109",
                crop: "Rice",
            },
        ],
        Dealer: [
            {
                name: "Agri Solutions",
                location: "Market Yard",
                phone: "+91 76543 21098",
                license: "DL-123",
            },
            {
                name: "Kisan Seva Kendra",
                location: "Main Road",
                phone: "+91 65432 10987",
                license: "DL-456",
            },
        ],
        Wholesaler: [
            {
                name: "Global Grains",
                location: "City Center",
                phone: "+91 54321 09876",
                gst: "GST-789",
            },
            {
                name: "Fresh Produce Co.",
                location: "Industrial Area",
                phone: "+91 43210 98765",
                gst: "GST-012",
            },
        ],
    };
</script>

<div class="contacts-page">
    <h1 class="page-title">Contacts</h1>

    <div class="tabs">
        {#each tabs as tab}
            <button
                class="tab-btn {activeTab === tab ? 'active' : ''}"
                on:click={() => (activeTab = tab)}
            >
                {tab}
            </button>
        {/each}
    </div>

    <div class="contacts-grid">
        {#each contactsData[activeTab] as contact}
            <GlassCard>
                <div class="contact-card">
                    <div class="avatar">{contact.name[0]}</div>
                    <div class="info">
                        <h3>{contact.name}</h3>
                        <p class="location">📍 {contact.location}</p>
                        <p class="phone">📞 {contact.phone}</p>
                        {#if contact.crop}
                            <span class="badge">🌾 {contact.crop}</span>
                        {/if}
                        {#if contact.license}
                            <span class="badge">📜 {contact.license}</span>
                        {/if}
                        {#if contact.gst}
                            <span class="badge">🏢 {contact.gst}</span>
                        {/if}
                    </div>
                </div>
            </GlassCard>
        {/each}
    </div>
</div>

<style>
    .contacts-page {
        padding: 2rem;
    }

    .page-title {
        font-size: 2rem;
        margin-bottom: 2rem;
        color: var(--text-main);
    }

    .tabs {
        display: flex;
        gap: 1rem;
        margin-bottom: 2rem;
        border-bottom: 1px solid var(--card-border);
        padding-bottom: 1rem;
    }

    .tab-btn {
        background: transparent;
        border: none;
        padding: 8px 16px;
        font-size: 1.1rem;
        color: var(--text-secondary);
        cursor: pointer;
        border-radius: 8px;
        transition: all 0.3s;
    }

    .tab-btn:hover {
        background: rgba(0, 0, 0, 0.05);
        color: var(--color-primary);
    }

    .tab-btn.active {
        background: var(--color-primary);
        color: white;
    }

    .contacts-grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
        gap: 1.5rem;
    }

    .contact-card {
        display: flex;
        align-items: center;
        gap: 1rem;
    }

    .avatar {
        width: 50px;
        height: 50px;
        background: var(--color-primary);
        color: white;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 1.5rem;
        font-weight: 700;
    }

    .info h3 {
        margin: 0 0 0.25rem 0;
        color: var(--text-main);
    }

    .info p {
        margin: 0 0 0.25rem 0;
        font-size: 0.9rem;
        color: var(--text-secondary);
    }

    .badge {
        display: inline-block;
        font-size: 0.8rem;
        padding: 2px 8px;
        background: rgba(0, 0, 0, 0.1);
        border-radius: 12px;
        margin-top: 0.5rem;
        color: var(--text-main);
    }
</style>
