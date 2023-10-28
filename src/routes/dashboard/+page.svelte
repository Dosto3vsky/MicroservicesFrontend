<script lang="ts">
	import AdCards from '$lib/components/AdCards';
	import axios from 'axios';
	import { onMount } from 'svelte';
	import Navbar from '$lib/components/Navbar';
	import DoubleRangeSlider from '$lib/shared/ui/DoubleRangeSlider';

	let searchValue = '';
    let searchType = 'book';

	let ads : any[] = [];

    let priceStart = 0;
    let priceEnd = 1;
    const priceFactor = 1000

    let timesRentedStart = 0;
    let timesRentedEnd = 1;
    const timesRentedFactor = 100;

    // Removes decimals and scales the value
    const formatSliderLabel = (value: number, factor: number) => {
        return (value*factor).toFixed(0);
    }

    // Handler for search event sent from navbar
    const search = async (event: CustomEvent) => {
        ads = (await axios.get(`https://api.rentwave.live/${searchType}/list/active`)).data;
        searchValue = event.detail;
        console.log(ads);
    }

    // Handler for type change sent from navbar
    const changeType = (event: CustomEvent) => {
        searchType = event.detail;
    }

	onMount(async () => {
        ads = (await axios.get(`https://api.rentwave.live/${searchType}/list/active`)).data;
        console.log(ads);
	});
</script>

<svelte:head>
	<title>Dashboard</title>
	<meta name="description" content="RentWave" />
</svelte:head>

<div class="root">
	<Navbar on:search={search} on:changeType={changeType} />

    <div class="content">

        <div class="leftbar">
            <div class="mb-16">
                <p class="leftbar-heading">Filters</p>

                <div class="filters">
                    <div class="price-filter">
                        <p class="filter-title">Price Filter</p>

                        <DoubleRangeSlider bind:start={priceStart} bind:end={priceEnd} />
                        <div class="labels">
                            <div><b>₹{formatSliderLabel(priceStart, priceFactor)}</b>/hr</div>
                            <div><b>₹{formatSliderLabel(priceEnd, priceFactor)}</b>/hr</div>
                        </div>

                    </div>

                    <div class="rental-filter">
                        <p class="filter-title">Times Rented</p>

                        <DoubleRangeSlider bind:start={timesRentedStart} bind:end={timesRentedEnd} />
                        <div class="labels">
                            <div><b>{formatSliderLabel(timesRentedStart, timesRentedFactor)}</b> nos</div>
                            <div><b>{formatSliderLabel(timesRentedEnd, timesRentedFactor)}</b> nos</div>
                        </div>

                    </div>
                </div>
                
            </div>

            <div>
                <p class="leftbar-heading">Top Rentals</p>

                <ul>
                    <li>1. <u>Harry Potter</u></li>
                    <li>2. <u>Percy Jackson</u></li>
                    <li>3. <u>Game Of Thrones</u></li>
                    <li>4. <u>The Witcher</u></li>

                </ul>
                
            </div>
        </div>

        <div class="search-result">
            {#if searchValue !== ''}
                <div class="search-result-heading">
                    <heading>Showing Results For <b>{searchValue}</b></heading>
                    <div>Showing 1-24 of 50,000 results</div>
                </div>
            {/if}

            <div class="grid grid-cols-5 gap-4">
                {#if ads.length > 0}
                    {#each ads as ad}
                        {#each ad.Details as detailItem}
                            <AdCards title={ad.Title} description={detailItem.Description}/>
                        {/each}
                    {/each}
                {:else}
                    No Results Found
                {/if}
                
            </div>
        </div>

    </div>
</div>

<style>
	.root {
		display: flex;
        flex-direction: column;
        width: 100%;
        min-height: 100%;
	}

    .content {
        display: flex;
        flex-direction: row;
        width: 100%;
        min-height: 100vh;
        color: black;
    }

    .leftbar {
        width: 25%;
        min-height: 100%;
        background-color: white;
        border-right: solid 1px #C1C1C1;
        padding: 1rem;
    }

    .search-result {
        width: 100%;
        min-height: 100%;
        background-color: white;
        padding: 3rem;
    }

    .search-result-heading {
        margin-bottom: 2rem;
    }

    .search-result-heading heading {
        font-size: 40px;
        font-weight: 500;
    }

    .search-result-heading div {
        font-weight: lighter;
        font-size: 0.8rem;
        color: grey;
    }

    .leftbar-heading {
        font-size: 32px;
        font-weight: bold;
        border-bottom: solid 1px rgba(0, 0, 0, 0.33);
        margin-bottom: 1rem;
    }

    .filters {
        padding-left: 0.5rem;
    }

    .filter-title {
        font-size: 26px;
        font-weight: bold;
        margin-bottom: 1rem;
    }

    .labels {
        width: 100%;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }
</style>
