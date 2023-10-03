<script lang="ts">
    import axios from 'axios'
    import Input from './Input.svelte'
    import Button from './Button.svelte'

    let name: string = ''
    let type: string = ''
    let desc: string = ''
    let price: string = ''
    let active: string = ''

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

        console.log(res)
    }
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
    </fieldset>
</div>