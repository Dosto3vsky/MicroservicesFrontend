<script lang="ts">
    import axios from 'axios'
    import Input from './Input.svelte'
    import Button from './Button.svelte'
    import { onMount } from 'svelte';

    let name: string = ''
    let type: string = ''
    let desc: string = ''
    let price: string = ''
    let active: string = ''
    let allListings = []
    let addedId: string = ''
    let toggleId = ''
    let userListings = []

    const onAdd = async () => {
        const res = await axios.post('https://api.rentwave.live/vehicle/add', {
            "name": name,
            "type": type,
            "desc": desc,
            "price": price,
            "active": active
        }, {
            withCredentials: true
        })

        name = ''
        type = ''
        desc = ''
        price = ''
        active = ''
        addedId = res.data
    }

    const onToggle = async () => {
        const res = await axios.patch('https://api.rentwave.live/vehicle/toggle', {
            "_id": toggleId
        })

        toggleId = ''
    }

    onMount(async () => {
        let res = await axios.get('https://api.rentwave.live/vehicle/list/active')
        allListings = res.data

        res = await axios.get('https://api.rentwave.live/vehicle/me',{
            withCredentials: true
        })
        userListings = res.data
    })
</script>

<div>
    <fieldset>
        <legend>Vehicle Service</legend>
        <h3>Add Vehicle</h3>
        <Input
            type = "text"
            label = "Name"
            bind:value = {name}
        />
        <Input
            type = "text"
            label = "Type"
            bind:value = {type}
        />
        <Input
            type = "text"
            label = "Desc"
            bind:value = {desc}
        />
        <Input
            type = "text"
            label = "Price"
            bind:value = {price}
        />
        <Input
            type = "text"
            label = "Active"
            bind:value = {active}
        />
        <Button on:click={onAdd}>Add</Button>
        <br>
        <div>Added Listing Id: {addedId}</div>
        <h3>Active Vehicle Listings</h3>
        {#each allListings as listing}
            <div>
                <p>{listing.name}</p>
                <p>{listing.type}</p>
                <p>UID: {listing.uid}</p>
                <p>${listing.price} per day</p>
                <p>{listing.desc}</p>
            </div>
        {/each}
        <br>
        <h3>Toggle Listing Activeness</h3>
        <Input
            type = "text"
            label = "Listing Id"
            bind:value = {toggleId}
        />
        <Button on:click={onToggle}>Toggle</Button>
        <br>
        <h3>Your Listings</h3>
       {#each userListings as listing}
            <div>
                <p>{listing.name}</p>
                <p>{listing.type}</p>
                <p>${listing.price} per day</p>
                <p>{listing.desc}</p>
                <p>Active: {listing.active}</p>
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