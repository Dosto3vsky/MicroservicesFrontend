<script lang="ts">
    import axios from 'axios'
    import Input from './Input.svelte'
    import Button from './Button.svelte'
    import { onMount } from 'svelte';

    let isbn: string = ''
    let title: string = ''
    let author: string = ''
    let image: string = ''
    let description: string = ''
    let price: number = null
    let allListings = []
    let toggleId: number = null
    let userListings = []

    const onAdd = async () => {
        const res = await axios.post('https://api.rentwave.live/book/add', {
            "isbn": isbn,
            "title": title,
            "author": author,
            "image": image,
            "description": description,
            "price": price
        }, {
            withCredentials: true
        })

        isbn = ''
        title = ''
        author = ''
        image = ''
        description = ''
        price = null
    }

    const onToggle = async () => {
        const res = await axios.patch('https://api.rentwave.live/book/toggle', {
            "_id": toggleId
        })

        toggleId = null
    }

    onMount(async () => {
        let res = await axios.get('https://api.rentwave.live/book/list/active')
        allListings = res.data


        res = await axios.get('https://api.rentwave.live/book/me',{
            withCredentials: true
        })
        userListings = res.data

    })
</script>

<div>
    <fieldset>
        <legend>Book Service</legend>
        <h3>Add Book</h3>
        <Input
            type = "text"
            label = "ISBN"
            bind:value = {isbn}
        />
        <Input
            type = "text"
            label = "Title"
            bind:value = {title}
        />
        <Input
            type = "text"
            label = "Author"
        bind:value = {author}
        />
        <Input
            type = "text"
            label = "Description"
            bind:value = {description}
        />
        <Input
            type = "text"
            label = "Image Link"
            bind:value = {image}
        />
        <Input
            type = "number"
            label = "Price"
            bind:value = {price}
        />
        <Button on:click={onAdd}>Add</Button>
        <br>
        <h3>Active Book Listings</h3>
        {#each allListings as listing}
            <div>
                <p>Listing Id: {listing.ID}</p>
            </div>
        {/each}
        <br>
        <h3>Toggle Listing Activeness</h3>
        <Input
            type = "number"
            label = "Listing Id"
            bind:value = {toggleId}
        />
        <Button on:click={onToggle}>Toggle</Button>
        <br>
        <h3>Your Listings</h3>
       {#each userListings as listing}
            <div>
                <p>Listing Id: {listing.ID}</p>
            </div>
       {/each} 
    </fieldset>
</div>

<style>
    div {
        display: inline-block;
        margin-right: 10px;
    }
</style>