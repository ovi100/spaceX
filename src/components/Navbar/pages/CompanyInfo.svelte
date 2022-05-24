<script>
	import { onMount } from 'svelte';
	import { fly } from 'svelte/transition';
	import Loder from './Loder.svelte';
	let loading = true;
	let company = [];

	onMount(async () => {
		const res = await fetch('https://api.spacexdata.com/v3/info');
		company = await res.json();
		console.log(company);
		loading = false;
	});
</script>

<svelte:head>
	<title>compay info</title>
</svelte:head>

{#if loading}
	<Loder />
{:else}
	<div
		class="overflow-hidden shadow-sm mx-auto my-8"
		in:fly={{ y: 50, duration: 500 }}
		out:fly={{ duration: 500 }}
	>
		<div class="grid grid-cols-2 gap-4">
			<div class="border-r border-gray-200 px-6 py-4">
				<div class="font-bold text-xl mb-2">{company.name}</div>
				<p class="text-gray-600 text-base mb-2">{company.summary}</p>
				<a href={company.links.website} target="_blank">More details</a>
			</div>
			<div class="px-6 py-4">
				<div class="font-bold text-xl p-4">Our Team</div>
				<div class="px-3 py-1 text-sm font-semibold text-gray-600 mb-2">
					{company.ceo}(CEO & Founder)
				</div>
				<div class="px-3 py-1 text-sm font-semibold text-gray-600 mb-2">
					{company.coo}(COO)
				</div>
				<div class="px-3 py-1 text-sm font-semibold text-gray-600 mb-2">
					{company.cto}(CTO)
				</div>
			</div>
		</div>
		<div class="px-4 pb-3">
			<div class="font-bold text-xl mb-1">Address:</div>
      <p class="mb-2">{company.headquarters.address},{company.headquarters.city},{company.headquarters.state}</p>
			<div class="font-bold text-xl mb-2">Find us on</div>
      <div class="flex">
        <a href={company.links.website} target="_blank">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6 stroke-gray-500 mr-2"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9"
            />
          </svg>
        </a>
        <a href={company.links.twitter} target="_blank">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6 stroke-gray-500 mr-2"
            fill="none"
            viewBox="0 0 512 512"
            stroke-width="10"
            ><path
              d="M346.8,80c22.3,0,43.8,9.3,59,25.6l6.1,6.5l8.7-1.7c4.5-0.9,8.9-1.9,13.3-3.1c-1,1.4-5.7,6.1-11,10.8  c-4.5,4-17.8,16.5-17.8,16.5s13,4.1,21.1,5s17.4-0.6,18.9-0.8c-2.8,2.3-7.8,5.8-10.8,7.9l-7,5.1l0.4,8.6c0.2,3.8,0.3,7.8,0.3,11.8  c0,30.2-5.9,61.8-17,91.5c-11.7,31.2-28.5,59.4-50,83.8c-23,26.1-50.2,46.5-81.1,60.8c-33.8,15.7-71.3,23.6-111.5,23.6  c-28.9,0-57.4-4.8-84.5-14.2c9.9-1.9,19.6-4.6,29.1-7.9c21.7-7.6,41.9-18.7,60.1-33l35.3-27.7l-44.9-0.8  c-26.1-0.5-49.7-13.4-64.3-33.9c7.3-0.5,14.5-1.8,21.5-3.7l60.7-20.2l-61.7-10.9c-29.6-5.9-52.8-27.9-61.3-55.8  c7.8,2,15.4,2.8,23.9,3.3c0,0,31.8,1.4,55.8-0.1c-13-6.2-46.4-29.2-46.4-29.2c-22.5-15.1-36-40.2-36-67.4c0-6.4,0.8-12.8,2.2-19  c21.9,22.4,47,41.1,75.1,55.5c37,19.1,76.9,29.8,118.6,31.9l21.2,1.1l-4.8-20.7c-1.4-5.9-2.1-12.1-2.1-18.5  C265.9,116.3,302.2,80,346.8,80 M346.8,64c-53.5,0-96.8,43.4-96.8,96.9c0,7.6,0.8,15,2.5,22.1C172,179,100.6,140.4,52.9,81.7  c-8.3,14.3-13.1,31-13.1,48.7c0,33.6,17.1,63.3,43.1,80.7C67,210.7,52,206.3,39,199c0,0.4,0,0.8,0,1.2c0,47,33.4,86.1,77.7,95  c-8.1,2.2-16.7,3.4-25.5,3.4c-6.2,0-12.3-0.6-18.2-1.8c12.3,38.5,48.1,66.5,90.5,67.3c-33.1,26-74.9,41.5-120.3,41.5  c-7.8,0-15.5-0.5-23.1-1.4C62.9,432,113.8,448,168.4,448C346.6,448,444,300.3,444,172.2c0-4.2-0.1-8.4-0.3-12.5  c18.9-13.7,35.3-30.7,48.3-50.2c-17.4,7.7-36,12.9-55.6,15.3c20-12,35.4-31,42.6-53.6c-18.7,11.1-39.4,19.2-61.5,23.5  C399.8,75.8,374.6,64,346.8,64L346.8,64z"
            /></svg
          >
        </a>
      </div>
		</div>
	</div>
{/if}
