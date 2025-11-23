<script>
    import { onMount, createEventDispatcher } from "svelte";
    import { theme } from "$lib/stores/theme";

    const dispatch = createEventDispatcher();

    let time = new Date();
    let weather = { temp: "--", condition: "Loading..." };
    let location = "Detecting...";

    onMount(() => {
        // Time Update
        const timer = setInterval(() => {
            time = new Date();
        }, 1000);

        // Weather Fetch
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(
                async (position) => {
                    const { latitude, longitude } = position.coords;
                    try {
                        const response = await fetch(
                            `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current_weather=true`,
                        );
                        const data = await response.json();
                        weather = {
                            temp: data.current_weather.temperature,
                            condition: getWeatherCondition(
                                data.current_weather.weathercode,
                            ),
                        };
                        location = "Local Weather"; // Could use reverse geocoding here if needed
                    } catch (e) {
                        console.error("Weather fetch failed", e);
                        weather = { temp: "--", condition: "Unavailable" };
                    }
                },
                (error) => {
                    console.error("Geolocation error", error);
                    location = "Location Denied";
                    weather = { temp: "--", condition: "-" };
                },
            );
        }

        return () => clearInterval(timer);
    });

    /** @param {number} code */
    function getWeatherCondition(code) {
        // Simplified WMO code mapping
        if (code === 0) return "Clear Sky";
        if (code >= 1 && code <= 3) return "Partly Cloudy";
        if (code >= 45 && code <= 48) return "Foggy";
        if (code >= 51 && code <= 67) return "Rainy";
        if (code >= 71 && code <= 77) return "Snowy";
        if (code >= 95) return "Thunderstorm";
        return "Unknown";
    }

    function toggleTheme() {
        theme.update((t) => (t === "light" ? "dark" : "light"));
    }
</script>

<header class="app-header">
    <div class="left-section">
        <div class="logo">CropzCard</div>
        <div class="weather-widget">
            <span class="location">{location}</span>
            <span class="temp">{weather.temp}°C</span>
            <span class="condition">{weather.condition}</span>
        </div>
    </div>

    <div class="right-section">
        <div class="time-display">
            {time.toLocaleTimeString([], {
                hour: "2-digit",
                minute: "2-digit",
            })}
        </div>

        <button class="icon-btn" on:click={toggleTheme} title="Toggle Theme">
            {#if $theme === "light"}
                🌙
            {:else}
                ☀️
            {/if}
        </button>

        <button
            class="icon-btn"
            on:click={() => dispatch("settings")}
            title="Settings"
        >
            ⚙️
        </button>
    </div>
</header>

<style>
    .app-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 1rem 2rem;
        background: var(--card-bg);
        backdrop-filter: blur(10px);
        border-bottom: 1px solid var(--card-border);
        position: sticky;
        top: 0;
        z-index: 50;
    }

    .left-section,
    .right-section {
        display: flex;
        align-items: center;
        gap: 1.5rem;
    }

    .logo {
        font-size: 1.5rem;
        font-weight: 800;
        color: var(--color-primary);
        letter-spacing: -0.5px;
    }

    .weather-widget {
        display: flex;
        flex-direction: column;
        font-size: 0.8rem;
        color: var(--text-secondary);
        line-height: 1.2;
    }

    .weather-widget .temp {
        font-weight: 700;
        color: var(--text-main);
        font-size: 0.9rem;
    }

    .time-display {
        font-family: monospace;
        font-size: 1.2rem;
        font-weight: 600;
        color: var(--text-main);
    }

    .icon-btn {
        background: transparent;
        border: none;
        font-size: 1.2rem;
        cursor: pointer;
        padding: 8px;
        border-radius: 50%;
        transition: background 0.3s;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .icon-btn:hover {
        background: rgba(0, 0, 0, 0.05);
    }

    @media (max-width: 600px) {
        .app-header {
            padding: 0.8rem 1rem;
        }
        .weather-widget {
            display: none;
        }
        .logo {
            font-size: 1.2rem;
        }
    }
</style>
