<script>
	import { fly } from 'svelte/transition';
	export let launch;
	let src;
	if (launch.links.mission_patch === null) {
		src = 'https://placehold.jp/3d4070/ffffff/248x248.jpg?text=image';
	}
	else if (
		launch.links.mission_patch === 'https://i.imgur.com/t5R4BAQ.png' ||
		launch.links.mission_patch === 'https://i.imgur.com/Ehe9AgY.png' ||
		launch.links.mission_patch === 'https://i.imgur.com/E7fjUBD.png' ||
		launch.links.mission_patch === 'https://imgur.com/E7fjUBD.png' 
	) {
		src = 'https://placehold.jp/3d4070/ffffff/248x248.jpg?text=image';
	} else {
		src = launch.links.mission_patch;
	}
</script>

<div
	class="border rounded overflow-hidden shadow-lg px-4 py-4"
	in:fly={{ y: 50, duration: 500 }}
	out:fly={{ duration: 500 }}
>
	<a href={launch.links.article_link} target="_blank">
		<img class="w-full" {src} alt={launch.mission_name} />
	</a>
	<div class="flex justify-between mt-3 mb-2">
		<div class="font-bold text-lg truncate mb-2">
			<a href={launch.links.article_link} target="_blank">{launch.mission_name}</a>
		</div>
		<div>{launch.launch_year}</div>
	</div>
	{#if launch.details != null}
		<p class="truncate text-gray-700 text-base">{launch.details}</p>
	{:else}
		<a href={launch.links.article_link} target="_blank">View details</a>
	{/if}
	<div class="pt-2">
		<span
			class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2"
			>#{launch.rocket.rocket_name}</span
		>
	</div>
</div>
