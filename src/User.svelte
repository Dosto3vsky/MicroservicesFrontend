<script lang="ts">
    import axios from 'axios'
    import Input from './Input.svelte'
    import Button from './Button.svelte'

    let new_user: string = ''
    let new_pass: string = ''
    let username: string = ''
    let password: string = ''

    const onRegister = async () => {
        const res = await axios.post('https://api.rentwave.live/user/register', {
            "username": new_user,
            "password": new_pass
        })

        console.log(res)
        new_user = ''
        new_pass = ''
    }

    const onLogin = async () => {
        const res = await axios.post('https://api.rentwave.live/user/login', {
            "username": username,
            "password": password
        }, {
            withCredentials: true
        })

        console.log(res)
        username = ''
        password = ''
    }
</script>

<div>
    <fieldset>
        <legend>User Service</legend>
        <h3>Register New User</h3>
        <Input
            type = "text"
            label = "Username"
            bind:value = {new_user}
        />
        <Input
            type = "text"
            label = "Password"
            bind:value = {new_pass}
        />
        <Button on:click={onRegister}>Register</Button>
        <br>
        <h3>Login</h3>
        <Input
            type = "text"
            label = "Username"
            bind:value = {username}
        />
        <Input
            type = "text"
            label = "Password"
            bind:value = {password}
        />
        <Button on:click={onLogin}>Login</Button>
    </fieldset>
</div>
