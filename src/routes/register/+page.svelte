<script>
    import GlassCard from '$lib/components/GlassCard.svelte';
    import Input from '$lib/components/Input.svelte';
    import { goto } from '$app/navigation';

    let formData = {
        username: '',
        contact: '',
        password: '',
        shopAddress: '',
        gstNumber: '',
        seedLicense: '',
        fertilizerLicense: ''
    };

    function handleSubmit() {
        // In a real app, this would send data to a backend.
        // For now, we'll just simulate a successful registration and login.
        console.log('Registering:', formData);
        localStorage.setItem('user', JSON.stringify(formData));
        goto('/dashboard');
    }
</script>

<div class="page-container">
    <div class="card-wrapper">
        <GlassCard>
            <h1 class="title">Farmer Registration</h1>
            <p class="subtitle">Join our agricultural network</p>

            <form on:submit|preventDefault={handleSubmit}>
                <Input id="username" label="User Name" bind:value={formData.username} required />
                <Input id="contact" label="Mobile Number or E-mail" bind:value={formData.contact} required />
                <Input id="password" type="password" label="Password" bind:value={formData.password} required />
                <Input id="shopAddress" label="Shop Address" bind:value={formData.shopAddress} required />
                
                <div class="row">
                    <Input id="gst" label="GST Number" bind:value={formData.gstNumber} required />
                    <Input id="seed" label="Seed License Number" bind:value={formData.seedLicense} required />
                </div>
                
                <Input id="fertilizer" label="Fertilizer License Number" bind:value={formData.fertilizerLicense} required />

                <button type="submit" class="submit-btn">Register</button>
            </form>

            <p class="footer-text">
                Already have an account? <a href="/login">Log in</a>
            </p>
        </GlassCard>
    </div>
</div>

<style>
    .page-container {
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
        padding: 2rem;
    }

    .card-wrapper {
        width: 100%;
        max-width: 600px;
    }

    .title {
        font-size: 2rem;
        font-weight: 700;
        text-align: center;
        margin-bottom: 0.5rem;
        color: var(--color-accent);
    }

    .subtitle {
        text-align: center;
        margin-bottom: 2rem;
        color: var(--color-text-dim);
    }

    .row {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 1rem;
    }

    @media (max-width: 600px) {
        .row {
            grid-template-columns: 1fr;
        }
    }

    .submit-btn {
        width: 100%;
        padding: 14px;
        background: var(--color-primary);
        color: white;
        border: none;
        border-radius: 8px;
        font-size: 1.1rem;
        font-weight: 600;
        cursor: pointer;
        transition: background 0.3s ease;
        margin-top: 1rem;
    }

    .submit-btn:hover {
        background: var(--color-primary-dark);
    }

    .footer-text {
        text-align: center;
        margin-top: 1.5rem;
        color: var(--color-text-dim);
    }

    .footer-text a {
        color: var(--color-accent);
        font-weight: 600;
    }

    .footer-text a:hover {
        text-decoration: underline;
    }
</style>
