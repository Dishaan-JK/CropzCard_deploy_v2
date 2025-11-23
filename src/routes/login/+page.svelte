<script>
    import GlassCard from "$lib/components/GlassCard.svelte";
    import Input from "$lib/components/Input.svelte";
    import GoogleBtn from "$lib/components/GoogleBtn.svelte";
    import { goto } from "$app/navigation";
    import { jwtDecode } from "jwt-decode";

    let formData = {
        name: "",
        contact: "",
        password: "",
        shopAddress: "",
        gstNumber: "",
        seedLicense: "",
        pesticideLicense: "",
        userType: "Farmer", // Default
    };

    const userTypes = ["Farmer", "Dealer", "Wholesaler"];

    function handleLogin() {
        console.log("Logging in:", formData);
        // Simulate login - save full profile
        localStorage.setItem(
            "user",
            JSON.stringify({
                ...formData,
                username: formData.name || "User",
            }),
        );
        goto("/dashboard");
    }

    /** @param {CustomEvent} event */
    function handleGoogleLogin(event) {
        const credential = event.detail;
        try {
            /** @type {any} */
            const decoded = jwtDecode(credential);
            console.log("Google User:", decoded);

            const googleUser = {
                name: decoded.name,
                contact: decoded.email,
                userType: "Farmer", // Defaulting to Farmer for Google Login
                username: decoded.name,
                picture: decoded.picture,
            };

            localStorage.setItem("user", JSON.stringify(googleUser));
            goto("/dashboard");
        } catch (error) {
            console.error("Login Failed", error);
        }
    }
</script>

<div class="page-container">
    <div class="card-wrapper">
        <GlassCard>
            <h1 class="title">CropzCard</h1>
            <p class="subtitle">Sign in to your account</p>

            <GoogleBtn on:login={handleGoogleLogin} />

            <div class="divider">
                <span>OR</span>
            </div>

            <form on:submit|preventDefault={handleLogin}>
                <div class="user-type-selector">
                    {#each userTypes as type}
                        <button
                            type="button"
                            class="type-btn {formData.userType === type
                                ? 'active'
                                : ''}"
                            on:click={() => (formData.userType = type)}
                        >
                            {type}
                        </button>
                    {/each}
                </div>

                <Input
                    id="name"
                    label="Full Name"
                    bind:value={formData.name}
                    required
                />
                <Input
                    id="contact"
                    label="Mobile Number or E-mail"
                    bind:value={formData.contact}
                    required
                />
                <Input
                    id="password"
                    type="password"
                    label="Password"
                    bind:value={formData.password}
                    required
                />
                <Input
                    id="shopAddress"
                    label="Shop Address"
                    bind:value={formData.shopAddress}
                    required
                />

                <div class="row">
                    <Input
                        id="gst"
                        label="GST Number"
                        bind:value={formData.gstNumber}
                        required
                    />
                    <Input
                        id="seed"
                        label="Seed License"
                        bind:value={formData.seedLicense}
                        required
                    />
                </div>

                <Input
                    id="pesticide"
                    label="Pesticide License"
                    bind:value={formData.pesticideLicense}
                    required
                />

                <button type="submit" class="submit-btn">Log In</button>
            </form>

            <p class="footer-text">
                Don't have an account? <a href="/register">Register here</a>
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
        max-width: 500px;
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
        margin-bottom: 1.5rem;
        color: var(--text-secondary);
    }

    .divider {
        display: flex;
        align-items: center;
        text-align: center;
        margin: 1.5rem 0;
        color: var(--text-secondary);
    }

    .divider::before,
    .divider::after {
        content: "";
        flex: 1;
        border-bottom: 1px solid var(--glass-border);
    }

    .divider span {
        padding: 0 10px;
        font-size: 0.9rem;
    }

    .user-type-selector {
        display: flex;
        gap: 0.5rem;
        margin-bottom: 1.5rem;
        background: rgba(0, 0, 0, 0.2);
        padding: 4px;
        border-radius: 10px;
    }

    .type-btn {
        flex: 1;
        padding: 8px;
        border: none;
        background: transparent;
        color: var(--text-secondary);
        border-radius: 8px;
        cursor: pointer;
        transition: all 0.3s ease;
        font-weight: 500;
    }

    .type-btn.active {
        background: var(--color-primary);
        color: white;
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
        color: var(--text-secondary);
    }

    .footer-text a {
        color: var(--color-accent);
        font-weight: 600;
    }

    .footer-text a:hover {
        text-decoration: underline;
    }
</style>
