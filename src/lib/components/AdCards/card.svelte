<script lang="ts">
    export let imageURL:string = "https://upload.wikimedia.org/wikipedia/en/d/dc/A_Song_of_Ice_and_Fire_book_collection_box_set_cover.jpg"
    export let title = "Advertisements"
    export let description = ""
    export let isbn = ""
    export let id;
</script>

<div class="ad-card">
    <img class="ad-image" src={imageURL} alt="Ad">
    <div class="ad-title">{title} - <span class="text-gray-500 font-normal">{isbn}</span></div>
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