<script>
	import { onMount } from 'svelte';
	import Loder from './Loder.svelte';
	import Card from '../pages/Card.svelte';
	let keywords = '';
	let filterOptions = ['All', 'Upcoming', 'Not upcoming', 'Successful', 'Unsuccessful'];
	let selectedFilter = filterOptions[0];
	let loading = true;
	let launchs = [];
	let filterLaunchs = [];
	let numberOfData;
	let dataLimit = 15;
	let isVisible = true;

	onMount(async () => {
		const res = await fetch('https://api.spacexdata.com/v3/launches');
		launchs = await res.json();
		filterLaunchs = launchs;
		//console.log(launchs);
		loading = false;
	});

	function searchData() {
		filterLaunchs = launchs;

		if (keywords && selectedFilter === 'All') {
			//selectedFilter = 'All';
			filterLaunchs = launchs.filter((launch) => {
				return launch.rocket.rocket_name.toLowerCase().includes(keywords.toLowerCase());
			});
		}
	}

	function loadMore() {
		numberOfData = launchs.length;
		dataLimit = dataLimit + 10;

		if (numberOfData <= dataLimit) {
			return (isVisible = false);
		}
		return dataLimit;
	}

	$: {
		if (selectedFilter) {
			//keywords = '';
			if (selectedFilter.toLowerCase() === 'all') {
				filterLaunchs = launchs;
			}
			if (selectedFilter.toLowerCase() === 'upcoming') {
				filterLaunchs = launchs.filter((data) => data.upcoming === true);
			}
			if (selectedFilter.toLowerCase() === 'not upcoming') {
				filterLaunchs = launchs.filter((data) => data.upcoming === false);
			}
			if (selectedFilter.toLowerCase() === 'successful') {
				filterLaunchs = launchs.filter((data) => data.launch_success === true);
			}
			if (selectedFilter.toLowerCase() === 'unsuccessful') {
				filterLaunchs = launchs.filter(
					(data) => data.launch_success === false || data.launch_success === null
				);
			}
		}
	}
</script>

<svelte:head>
	<title>Launch</title>
</svelte:head>
{#if loading}
	<Loder />
{:else}
	<div class="shadow overflow-hidden w-full pb-4">
		<div class="w-full flex justify-between px-4 pt-5 bg-white">
			<div class="w-full flex mt-1 rounded-md">
				<div class="search-box group relative">
					<svg
						width="20"
						height="20"
						fill="currentColor"
						class="absolute left-3 top-1/2 -mt-2.5 text-slate-400 pointer-events-none group-focus-within:text-black-300"
						aria-hidden="true"
					>
						<path
							fill-rule="evenodd"
							clip-rule="evenodd"
							d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
						/>
					</svg>
					<input
						autocomplete="off"
						class="focus:ring-2 focus:ring-black-300 focus:outline-none appearance-none w-full text-sm leading-6 text-slate-900 placeholder-slate-400 rounded-md py-2 pl-10 ring-1 ring-slate-200 shadow-sm"
						type="text"
						placeholder="search by roket name"
						bind:value={keywords}
						on:keyup={searchData}
					/>
				</div>
				<div class="keywords">
					{#if keywords != ''}
						<p class="px-4 mt-3">
							You Type "{keywords.charAt(0).toUpperCase() + keywords.slice(1)}"
						</p>
					{/if}
				</div>
			</div>
			<div class="Filters">
				<select
					bind:value={selectedFilter}
					class="mt-1 block w-auto py-2 pl-3 pr-7 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
				>
					{#each filterOptions as option}
						<option value={option}>
							{option}
						</option>
					{/each}
				</select>
			</div>
		</div>
		<div class="grid grid-cols-5 gap-4 mt-5 px-4">
			{#if filterLaunchs.length > 0}
				{#each filterLaunchs.slice(0, dataLimit) as launch}
					<Card {launch} />
				{/each}
			{:else}
				<p class="font-sans text-lg font-bold">No result found</p>
			{/if}
		</div>
		{#if isVisible}
			<div class="text-center mt-5 p-4">
				<button
					on:click={loadMore}
					class="text-center bg-indigo-600 border border-transparent rounded-md py-3 px-4 font-medium text-white hover:bg-indigo-700"
				>
					Load more
				</button>
			</div>
		{/if}
	</div>
{/if}
