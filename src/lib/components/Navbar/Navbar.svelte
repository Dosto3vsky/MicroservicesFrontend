<script lang="ts">
	import Logo from '$lib/images/logo.jpeg';
    import NotificationBell from '$lib/images/Notification Bell.png'
    import Profile from '$lib/images/Profile.png'
    import Search from '$lib/images/Search.png'
	import { createEventDispatcher } from 'svelte';

    let searchType  = 'book';
    let searchValue = '';

    const dispatch = createEventDispatcher();

    // Called when search button is clicked
    function onSearch() {
        dispatch('search', searchValue);
    }

    // Called when search type changes in the dropdown
    function onTypeChange() {
        dispatch('changeType', searchType);
    }
</script>

<div class="navbar">
    <div class="logo">
		<img src={Logo} alt="logo" width="32px" height="32px" />
        <span>RentWave</span>
	</div>

    <div class="search-bar">

        <select class="dropdown" name="type" bind:value={searchType} on:change={onTypeChange}>
            <option value="book">Books</option>
            <option value="vehicle">Vehicles</option>
        </select>

        <div class="search-text">

            <input placeholder={`Search for a ${searchType}`} bind:value={searchValue} />
            <img src={Search} alt="Search" on:click|preventDefault={onSearch} />

        </div>

    </div>

    <div class="actions">
        <img src={NotificationBell} alt="Notifications" />
        <img src={Profile} alt="Notifications" />
    </div>
</div>

<style>
    .navbar {
        display: flex;
        background: linear-gradient(180deg, #013A55 0%, #00243C 100%);
        width: 100%;
        align-items: center;
        justify-content: space-between;
        padding: 0.25rem;
    }

    .logo {
        display: flex;
        color: #FFF;
        text-align: center;
        font-size: 28px;
        font-style: normal;
        font-weight: 700;
        line-height: normal;
        letter-spacing: 2.56px;
        align-items: center;
    }

    .logo img {
        cursor: pointer;
        width: 3rem;
        height: 3rem;
    }

    .actions {
        display: flex;
        align-items: center;
    }

    .actions img {
        cursor: pointer;
        width: 2rem;
        height: 2rem;
        margin: 0px 1rem 0px 1rem;
    }

    .search-bar {
        display: flex;
        align-items: center;
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
    }

    .search-text input:focus{
        outline: none;
    }

    .search-text img {
        cursor: pointer;
        width: 36px;
        height: 36px;
    }

    .dropdown {
        font-size: 20px;
        cursor: pointer;
        background-color: #DBDBDB;
        padding: 0.25rem 0.5rem 0.25rem 0.5rem;
        color: black;
        border-radius: 6px 0 0 6px;
        min-height: 36px;
    }

</style>