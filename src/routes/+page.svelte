<script lang="ts">
	import { onMount } from 'svelte';
	import mapboxgl from 'mapbox-gl';
	import 'mapbox-gl/dist/mapbox-gl.css';

	let map: mapboxgl.Map;
	let mapRef: HTMLDivElement;
	let lat = -74.5;
	let lng = 40;
	let zoom = 9;

	onMount(() => {
		if (!mapRef) return;

		mapboxgl.accessToken = import.meta.env.VITE_MAPBOX_ACCESS_TOKEN;

		map = new mapboxgl.Map({
			container: mapRef,
			style: 'mapbox://styles/mapbox/streets-v12',
			center: [lat, lng],
			zoom
		});

		map.on('move', () => {
		  const center = map.getCenter();

			lat = center.lat;
			lng = center.lng;
			zoom = map.getZoom();
		});
	});
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
	<link href="https://api.mapbox.com/mapbox-gl-js/v3.4.0/mapbox-gl.css" rel="stylesheet" />
</svelte:head>

<div class="container">
	<div class="map-wrapper">
		<div class="sidebar">
			Longitude: {lng.toFixed(4)} | Latitude: {lat.toFixed(4)} | Zoom:
			{zoom.toFixed(2)}
		</div>
		<div bind:this={mapRef} class="map"></div>
	</div>
</div>

<style lang="scss">
	.container {
		display: flex;
		flex: 1;
		border: 1px solid red;
	}

	.map-wrapper {
		position: relative;
		flex: 1;
	}

	.map {
	  width: 100%;
    height: 100%
	}

	.sidebar {
		background-color: rgb(35 55 75 / 90%);
		color: #fff;
		padding: 6px 12px;
		font-family: monospace;
		z-index: 1;
		position: absolute;
		top: 0;
		left: 0;
		margin: 12px;
		border-radius: 4px;
	}
</style>
