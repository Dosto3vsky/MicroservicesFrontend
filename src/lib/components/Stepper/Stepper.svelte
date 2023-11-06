<script lang="ts">
    export let steps = 1;
    export let activeStep;



</script>

<div class="stepper">
    {#each Array(steps) as _, index}
        <button class={`step ${index == activeStep ? 'active' : ''} ${activeStep > index ? 'complete' : ''}`} on:click={() => activeStep = index}>
            {index + 1}
        </button>
    {/each}
</div>

<style>
    .stepper {
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
        width: 20%;
        height: 70vh;
    }

    .step {
        width: 2rem;
        height: 2rem;
        background-color: white;
        font-weight: bold;
        border: solid 1px black;
        color: black;
        text-align: center;
        border-radius: 7px;
    }

    .active {
        background-color: #97B2D1;
    }

    .complete {
        background-color: #55CC52;
        color: white;
    }
</style>