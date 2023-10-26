<script lang="ts">
	import Button from "$lib/shared/ui/Button/Button.svelte";
	import Input from "$lib/shared/ui/Input/Input.svelte";
	import logo from "$lib/images/favicon.jpeg";
	import axios, { type AxiosResponse } from "axios"

	let username : string = ""
	let password : string = ""

    let error = "";


	$: console.log(username)
	$: console.log(password)

	async function LoginHandler(event : Event) {
		event.preventDefault();

		error = ""

		let res: AxiosResponse

		try {
			res = await axios.post("https://api.rentwave.live/user/login", {
				username: username,
				password: password
			}, {
				withCredentials: true,
			});
		}
		catch(e: unknown) {
			if (axios.isAxiosError(e))
                error = e.response?.data.error;
		}

	}

</script>

<svelte:head>
	<title>RentWave</title>
	<meta name="description" content="RentWave" />
</svelte:head>

<div class="root">
	<div class="flex flex-col items-center">
		<img src={logo} alt="logo" width="96px" height="96px" />
		<p class="mt-4 mb-8 text-3xl">RentWave</p>
		<form on:submit={LoginHandler} class="container">

			<div class="flex flex-col mb-8">
				<label for="username-input" class="font-bold text-2xl">Username</label>
				<Input bind:value={username} placeholder="john_doe" id="username-input" />
			</div>

			<div class="flex flex-col mb-16">
				<label for="password-input" class="font-bold text-2xl">Password</label>
				<Input bind:value={password} type="password" placeholder="**********" id="username-input" />
			</div>

			<Button class="mb-4" type="submit">Login</Button>

			{#if error != ""}
                <p class="text-red-600 text-sm mx-auto my-2">{error}</p>
            {/if}

			<a class="text-red-300 mx-auto mb-4" href="#top">Forgot Password?</a>

			<a class="text-slate-500 mx-auto" href="/register">Don't have an account?</a>

		</form>
	</div>
</div>

<style>
	.root {
		min-height: 100vh;
		background: linear-gradient(180deg, rgba(28, 28, 225, 0) 0%, rgba(0, 0, 26, 0.87) 77.08%), linear-gradient(0deg, rgba(6, 12, 71, 0.3), rgba(6, 12, 71, 0.3)), url("$lib/images/background.jpeg");
		background-size: cover;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.container {
		width: 650px;
		height: 550px;
		border-radius: 35px;
		background: linear-gradient(180deg, #030723 0%, rgba(3, 7, 35, 0.00) 100%);
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
		padding: 4rem 8rem 4rem 8rem;
	}
	
</style>