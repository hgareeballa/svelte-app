<!-- <script context="module">
	export async function load({ page }) {
		var songId = page.params.songId;
		const itunesSearched = await fetch(
			`https://itunes.apple.com/search?term=${songId}&entity=song`
		);
		var res = await itunesSearched.json();
		var songResults = res.results[0];
		return { props: { songResults } };
	}
</script> -->
<script>
	import { page } from '$app/stores';
	import Loading from '$lib/Loading.svelte';

	let songResults = [];
	let loading = true;

	const getSong = async () => {
		var songId = $page.params.songId;
		const itunesSearched = await fetch(
			`https://itunes.apple.com/search?term=${songId}&entity=song`,
			{
				mode: 'cors'
			}
		);
		var res = await itunesSearched.json();
		songResults = res.results[0];
		loading = false;
	};
	getSong();
</script>

<section>
	{#if !loading}
		<div class="mt-12 flex flex-col items-center justify-center">
			<h1 class="text-3xl font-bold text-center mb-12">{songResults.trackName}</h1>
			<img src={songResults.artworkUrl100} alt="img" class="w-1/4 rounded-md mb-12" />
			<audio controls>
				<source src={songResults.previewUrl} type="audio/mpeg" />
			</audio>
		</div>
	{:else}
		<Loading msg="Loading...!" />
	{/if}
</section>
