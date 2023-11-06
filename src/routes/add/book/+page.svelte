<script lang="ts">
	import { goto } from '$app/navigation';
	import { PUBLIC_GATEWAY_URL } from '$env/static/public';
	import Navbar from '$lib/components/Navbar/Navbar.svelte';
	import Stepper from '$lib/components/Stepper/Stepper.svelte';
	import Search from '$lib/images/Search.png';
	import Button from '$lib/shared/ui/Button/Button.svelte';
	import axios from 'axios';
	import { onMount } from 'svelte';

	let activeStep = 0;
	let bookData: any = {};

    let cloudinary: any;

	$: console.log(bookData);

	const searchIsbn = async () => {
		const res = await axios.get(`${PUBLIC_GATEWAY_URL}/book/isbn`, {
			params: {
				isbn: bookData.isbn
			}
		});

		if (res.data.found) {
			bookData = res.data.book;
			activeStep = 2;
		} else {
			activeStep = 1;
		}
	};

    const postAd = async () => {
        const res = await axios.post(`${PUBLIC_GATEWAY_URL}/book/add`, {
            ...bookData,
            price: bookData.price as number
        }, {
            withCredentials: true
        });

        goto("/dashboard");

    }

	onMount(() => {
		if ('cloudinary' in window) {
            //@ts-ignore
			cloudinary = window.cloudinary.createUploadWidget(
				{
					cloudName: 'dpklfec0x',
					uploadPreset: 'ml_default'
				},
				(error: any, result: any) => {
					if (!error && result && result.event === 'success') {
						console.log('Done! Here is the image info: ', result.info);
                        bookData.image = result.info.secure_url;
					}
				}
			);
		}
	});
</script>

<svelte:head>
	<title>Add Book</title>
</svelte:head>

<div class="root">
	<Navbar searchBarVisible={false} />

	<div class="content">
		<Stepper steps={3} bind:activeStep />

		<div class="form-content">
			{#if activeStep === 0}
				<div class="search-book">
					<div class="search-bar">
						<h1 class="text-3xl font-bold text-black">Enter ISBN Of The Book</h1>
						<div class="search-text">
							<input placeholder={`Ex. 9876543210123`} bind:value={bookData.isbn} />
							<button on:click={searchIsbn}><img src={Search} alt="Search" /></button>
						</div>
					</div>
				</div>
			{:else if activeStep === 1}
				<div class="new-book">
					<h1 class="text-3xl font-bold text-black mb-32">Add A New Book</h1>

					<form class="new-book-form">
						<div class="flex flex-col mb-8">
							<label for="isbn" class="text-xl font-bold text-black">ISBN</label>
							<input id="isbn" bind:value={bookData.isbn} />
						</div>

						<div class="flex flex-col mb-8">
							<label for="title" class="text-xl font-bold text-black">Title</label>
							<input id="title" bind:value={bookData.title} />
						</div>

						<div class="flex flex-col mb-8">
							<label for="author" class="text-xl font-bold text-black">Author</label>
							<input id="author" bind:value={bookData.author} />
						</div>

						<Button class="mb-8" on:click={() => activeStep++}>Next</Button>
					</form>
				</div>
			{:else}
				<div class="new-rental">
					<h1 class="text-3xl font-bold text-black mb-32">
						Post New Ad For {bookData.title ?? ''}
					</h1>

					<div class="new-rental-content">
						<div class="flex flex-col text-black w-1/2 mr-8">

							<div class="flex flex-col mb-8">
								<label for="price" class="text-xl font-bold text-black">Price</label>
								<input id="price" bind:value={bookData.price} />
							</div>

							<div class="flex flex-col mb-8">
								<label for="description" class="text-xl font-bold text-black">Desciption</label>
								<textarea rows="8" id="description" bind:value={bookData.description} />
							</div>

                            <button class="submit-btn" on:click={postAd}>Submit</button>
						</div>

						<div class="image-input" >
                            {#if bookData.image === undefined}
                                <button on:click={() => cloudinary.open()}>Choose an Image</button>
                            {:else}
                                <img src={bookData.image} alt="" />
                            {/if}
                        </div>
					</div>
				</div>
			{/if}
		</div>
	</div>
</div>

<style>
	.root {
		display: flex;
		flex-direction: column;
		width: 100%;
		min-height: 100vh;
		background-color: white;
	}

	.content {
		display: flex;
		justify-content: space-between;
		align-items: center;
		width: 100%;
		min-height: 100vh;
		padding: 1rem;
	}

	.form-content {
		display: flex;
		flex-direction: column;
		justify-content: center;
		width: 100%;
		height: 80vh;
	}

	.search-book {
		display: flex;
		flex-direction: column;
		justify-content: flex-start;
		align-items: center;
		text-align: left;
	}

	.search-bar {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		width: 33%;
	}

	.search-text {
		display: flex;
		width: 100%;
	}

	.search-text input {
		width: 100%;
		color: black;
		font-size: 20px;
		font-weight: 500;
		padding-left: 1rem;
		border: solid 1px black;
		border-right: none;
		border-radius: 7px;
		border-top-right-radius: 0;
		border-bottom-right-radius: 0;
	}

	.search-text input:focus {
		outline: none;
	}

	.search-text img {
		cursor: pointer;
		width: 36px;
		height: 36px;
		border: solid 1px black;
		border-left: none;
		border-radius: 7px;
		border-top-left-radius: 0;
		border-bottom-left-radius: 0;
	}

	.new-book {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		height: 100%;
	}

	.new-book-form {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		height: 50%;
	}

	.new-book-form input {
		border: solid 1px black;
		color: black;
		width: 15rem;
		font-size: 20px;
		padding: 3px;
		border-radius: 5px;
	}

	.new-book-form input:focus {
		outline: none;
	}

	.new-rental {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		height: 100%;
	}

	.new-rental-content {
		width: 100%;
		display: flex;
		justify-content: flex-start;
	}

	.new-rental-content input {
		border: solid 1px black;
		color: black;
		width: 15rem;
		font-size: 20px;
		padding: 3px;
		border-radius: 5px;
	}

    .new-rental-content textarea {
		border: solid 1px black;
		color: black;
		width: 25rem;
		font-size: 20px;
		padding: 3px;
		border-radius: 5px;
	}

	.image-input {
		background-color: #d9d9d9;
		border-radius: 8px;
		width: 25rem;
        height: 25rem;
		margin-right: 4rem;
        display: flex;
        justify-content: center;
        align-items: center;
	}

    .image-input button {
        color: black;
        border: solid 1px black;
        padding: 4px;
        border-radius: 5px;
    }

    /* .image-input img {
        width: 100%;
        height: 100%;
    } */

    .submit-btn {
        width: 30rem;
        height: 3rem;
        border-radius: 28px;
        background: linear-gradient(90deg, #238CEE 0%, #2337EE 100%);
        transition-duration: 300ms;
        color: white;
    }
</style>
