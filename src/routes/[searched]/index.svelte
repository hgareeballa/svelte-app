<script>
	import { page } from '$app/stores';
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';
	import Loading from '$lib/Loading.svelte';

	let songResults = [];
	let loading = true;

	onMount(async () => {
		var searched = $page.params.searched;
		const itunesSearched = await fetch(
			`https://itunes.apple.com/search?term=${searched}&entity=song`,
			{
				mode: 'cors'
			}
		);
		var res = await itunesSearched.json();
		songResults = res.results;
		loading = false;
		console.log(songResults, loading);
	});
</script>

<section>
	<div class="grid grid-cols-2 md:grid-cols-3 grod-flow row gap-3">
		{#each songResults as song}
			<button
				on:click={goto(`${$page.params.searched}/${song.trackId}`)}
				class="p-3 flex bg-white rounded-md bg-opacity-20 border-2 border-white border-opacity-30 hover:bg-blue-300 transition-all"
			>
				<img src={song.artworkUrl100} alt="img" class="rounded-md mr-4 w-1/4" />
				<div class="flex flex-col items-start text-left">
					<div class="mb-2 h-6 overflow-hidden">{song.trackName}</div>
					<div class="text-xs font-bold">{song.artistName}</div>
				</div>
			</button>
		{/each}

		{#if loading}
			<Loading msg="Loading...!" />
		{:else if songResults.length == 0}
			<Loading msg="No Results Found...!" />
		{/if}
	</div>
</section>
