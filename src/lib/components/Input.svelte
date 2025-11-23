<script>
    export let type = 'text';
    export let label = '';
    export let value = '';
    export let id = '';
    export let required = false;
</script>

<div class="input-group">
    <input 
        {type} 
        {id} 
        bind:value 
        {required} 
        placeholder=" "
    />
    <label for={id}>{label}</label>
</div>

<style>
    .input-group {
        position: relative;
        margin-bottom: 1.5rem;
    }

    input {
        width: 100%;
        padding: 12px 16px;
        background: rgba(255, 255, 255, 0.05);
        border: 1px solid var(--glass-border);
        border-radius: 8px;
        color: var(--color-text-light);
        font-size: 1rem;
        outline: none;
        transition: all 0.3s ease;
    }

    input:focus {
        border-color: var(--color-accent);
        background: rgba(255, 255, 255, 0.1);
    }

    label {
        position: absolute;
        left: 16px;
        top: 50%;
        transform: translateY(-50%);
        color: var(--color-text-dim);
        pointer-events: none;
        transition: all 0.3s ease;
        font-size: 1rem;
    }

    input:focus + label,
    input:not(:placeholder-shown) + label {
        top: -10px;
        left: 10px;
        font-size: 0.8rem;
        color: var(--color-accent);
        background: var(--color-bg-dark); /* Or transparent if careful */
        padding: 0 4px;
        border-radius: 4px;
    }
</style>
