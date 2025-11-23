<script>
    import { onMount } from "svelte";

    /** @type {HTMLCanvasElement} */
    let canvas;

    class Particle {
        /**
         * @param {number} width
         * @param {number} height
         */
        constructor(width, height) {
            this.x = Math.random() * width;
            this.y = Math.random() * height;
            this.vx = (Math.random() - 0.5) * 0.5;
            this.vy = (Math.random() - 0.5) * 0.5;
            this.size = Math.random() * 3 + 1;
            this.color =
                Math.random() > 0.5
                    ? "rgba(5, 150, 105, 0.3)"
                    : "rgba(217, 249, 157, 0.2)";
        }

        /**
         * @param {number} width
         * @param {number} height
         */
        update(width, height) {
            this.x += this.vx;
            this.y += this.vy;

            if (this.x < 0) this.x = width;
            if (this.x > width) this.x = 0;
            if (this.y < 0) this.y = height;
            if (this.y > height) this.y = 0;
        }

        /**
         * @param {CanvasRenderingContext2D} ctx
         */
        draw(ctx) {
            ctx.fillStyle = this.color;
            ctx.beginPath();
            ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
            ctx.fill();
        }
    }

    onMount(() => {
        const ctx = canvas.getContext("2d");
        if (!ctx) return;

        let width = 0;
        let height = 0;
        /** @type {Particle[]} */
        let particles = [];

        function resize() {
            width = window.innerWidth;
            height = window.innerHeight;
            canvas.width = width;
            canvas.height = height;
        }

        function init() {
            particles = [];
            for (let i = 0; i < 50; i++) {
                particles.push(new Particle(width, height));
            }
        }

        function animate() {
            if (!ctx) return;
            ctx.clearRect(0, 0, width, height);

            // Draw gradient background
            const gradient = ctx.createLinearGradient(0, 0, width, height);
            gradient.addColorStop(0, "#0f172a");
            gradient.addColorStop(1, "#064e3b");
            ctx.fillStyle = gradient;
            ctx.fillRect(0, 0, width, height);

            particles.forEach((p) => {
                p.update(width, height);
                p.draw(ctx);
            });

            requestAnimationFrame(animate);
        }

        window.addEventListener("resize", () => {
            resize();
            init();
        });

        resize();
        init();
        animate();
    });
</script>

<canvas bind:this={canvas} class="fixed top-0 left-0 w-full h-full -z-10"
></canvas>

<style>
    canvas {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1;
    }
</style>
