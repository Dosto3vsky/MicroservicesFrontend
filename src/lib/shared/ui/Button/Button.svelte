<script lang="ts">
    let klass = "";
    export {klass as class};
</script>

<button
    class=" {klass}"
    {...$$restProps}
>
    <slot />
</button>

<style>
    button {
        width: 100%;
        height: 3rem;
        border-radius: 28px;
        background: linear-gradient(90deg, #238CEE 0%, #2337EE 100%);
        transition-duration: 300ms;
    }

    button:active {
        transform: scale(0.95);
    }
</style>