<script>
	import { onMount } from 'svelte';
	import Loder from './Loder.svelte';
	let launchpads = [];

	onMount(async () => {
		const res = await fetch('https://api.spacexdata.com/v3/launchpads');
		launchpads = await res.json();
		setTimeout(() => {
			getCordinates(launchpads);
		}, 2000);
	});

	let map;

	function initMap() {
		map = new google.maps.Map(document.getElementById('map'), {
			zoom: 1,
			center: new google.maps.LatLng(-34.397, 150.644),
			mapTypeId: 'roadmap'
		});
	}

	// Loop through the results array and place a marker for each
	// set of coordinates.
	function getCordinates(data) {
		console.log(data);
		for (let i = 0; i < data.length; i++) {
			//console.log(data.location);
			const latLng = new google.maps.LatLng(data[i].location.latitude, data[i].location.longitude);
			new google.maps.Marker({
				position: latLng,
				map: map
			});
		}
	}

	window.initMap = initMap;
	//window.getCordinates = getCordinates;
</script>

<svelte:head>
	<title>Launchpads</title>
	<script
		defer
		src="https://maps.googleapis.com/maps/api/js?key=AIzaSyDZCjf3Wia-0yv2Is7CpSHzOj_4AJfScWA&callback=initMap"></script>
</svelte:head>

<div id="map" class="w-full" />

<style>
	#map {
		height: 600px;
	}
</style>
