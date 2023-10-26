<script lang="ts">
export let imageURL:string = ""
export let title:string = "Advertisements"
export let description:string = ""
</script>

<div class="ad-card">
    <img class="ad-image" src={imageURL} alt="Ad">
    <div class="ad-title">{title}</div>
    <div class="ad-details">{description}</div>
</div>

<style>
     .ad-card {
        background-color: #f9f9f9;
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
    }

    .ad-image {
        max-width: 100%;
        min-width: 20%;
        aspect-ratio: 1 / 1;    
    }

    .ad-title {
        font-weight: bold;
        color: black;
    }

    .ad-details {
        color: #555;
    }
</style>