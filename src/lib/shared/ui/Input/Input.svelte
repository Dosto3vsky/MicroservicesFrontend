<script lang="ts">
    export let placeholder : string = "";
    export let value : string = "";
    let klass : string = "";
    export {klass as class};

    // $: console.log(value)

</script>

<input 
    class=" p-2 text-xl {klass}" 
    {placeholder} 
    bind:value
    {...$$restProps}
/>

<style>
    input {
        background-color: #00000000;
        border-bottom: 2px solid #FFF;
    }

    input:focus {
        outline: none;
        border-bottom: 0;
    }
</style>