<script lang="ts">
    import { onMount } from "svelte";

    export let type: string = 'text';
    export let value;
    export let error: string = '';
    export let label: string = '';
    export let placeholder: string = '';

    onMount(() => {
      if (type === 'text')
      {
        value = '' as string
      }
      else
      {
        value = null as number
      }
    })

    function handleInput({ target: t }) {
        if (type === 'number') {
        value = (t.value === '' ? null : t.valueAsNumber);
        } else {
        value = t.value;
        }
    }
</script>

<label class="block">
    {#if label}
		<span class="block">{label}</span>
	{/if}
	<input
    class="block input"
    class:error
		{type}
		{placeholder}
		{value}
		on:input={handleInput}
		on:input
		on:blur
	/>
	{#if error}
		<span class="block error-text">{error}</span>
	{/if}
</label>

<style>
	.input {
		border-radius: 4px;
		padding: 6px 10px;
		margin: 0;
	}
	
	.block {
    display: block;
  }

  .error {
    border-color: #f55;
  }

  .error-text {
    color: #f55;
  }
</style>