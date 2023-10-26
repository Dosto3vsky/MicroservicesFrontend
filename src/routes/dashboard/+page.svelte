<script lang="ts">
	import AdCards from "$lib/components/AdCards";
	import axios from "axios";
    import { onMount } from "svelte";
    let base = "https://api.rentwave.live";
    let username = "John Doe"
let searchInput = ""; 
let books:any[] = [];
var myWidget:any;

let openCloudinary = ()=>{
    myWidget.open();
    console.log("click")
}
onMount(async ()=>{
    books = (await axios.get(base + "/book/list/active")).data
    console.log(books)
    if("cloudinary" in window){
    //@ts-ignore
    myWidget = window.cloudinary.createUploadWidget({
    cloudName: 'dpklfec0x', 
    uploadPreset: 'ml_default'}, (error:any, result:any) => { 
        if (!error && result && result.event === "success") { 
        console.log('Done! Here is the image info: ', result.info); 
        }
    }
    )
}
})

</script>

<div class="container">
<div class="header">
    <div class="user-info">
        <span>Welcome, {username}</span>
        <div>
        <input placeholder="Search for Ads" bind:value={searchInput}/>
        <button class="Searchbtn"> Search</button>
     </div>
     <button class="Logoutbtn"> Logout</button>
    </div>
</div>

<div class="dashboard">
    <div class="ad-grid">
        <!-- Add more ad-cards for additional advertisements -->
        {#each books as book}
            {#each book.Details as detailItem}
		        <AdCards title={book.Title} description={detailItem.Description}/>
            {/each}
	    {/each}
    </div>

</div>

<button class="upload-button" on:click={openCloudinary}>Upload Your Ad</button>
</div>

<style>
    .container {
        font-family: Arial, sans-serif;
        background-color: #f0f0f0;
        max-width: 100vw;
        min-height: 100vh;
        display: flex;
        flex-direction: column;
    }
    .Logoutbtn {
        background-color: #0056b3;
        padding-inline: 10px;
        border-radius: 3px;
        font-size: 1.3rem;
    }
    .header {
        background-color: #007BFF;
        color: #fff;
        padding: 10px;
        text-align: right;
    }

    .dashboard {
        margin: 20px auto;
        background-color: #fff;
        padding: 20px;
        width: 100%;

        border-radius: 5px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    .user-info {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .ad-grid {
        display: grid;
        grid-template-columns: repeat(5, minmax(0, 1fr));
        grid-gap: 20px;
        width: 100%;
        
    }

   

    .upload-button {
        background-color: #007BFF;
        color: #fff;
        border: none;
        padding: 10px 20px;
        border-radius: 5px;
        margin-top: auto;
        cursor: pointer;
        margin-left: auto;
        margin-right: 20px;
        margin-bottom: 20px;
    }

    .upload-button:hover {
        background-color: #0056b3;
    }

</style>