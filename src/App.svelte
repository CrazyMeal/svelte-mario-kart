<!-- Using https://github.com/rob-balfre/svelte-select -->
<script>
	import Select from 'svelte-select';

	const tracks = [
        'Champidrome', 'Parc Glougloop', 'Piste aux délices', 'Temple Thwomp',
        'Circuit Mario', 'Promenade Toad', 'Manoir Trempé', 'Cascades Maskass',
        'Aéroport Azur', 'Lagon Tourbillon', 'Club Mario', 'Descente givrée',
        'Voie céleste', 'Désert Toussec', 'Château de Bowser', 'Route Arc-en-ciel',
        'GCN Circuit Yoshi', 'Arène d\'Excitebike', 'Route du dragon', 'Mute City',
        'GCN Parc Baby', 'GBA Pays Fromage', 'Passage Feuillage', 'Animal Crossing',
        'Wii Prairie Meuh Meuh', 'GBA Circuit Mario', 'DS Plage Cheep Cheep', 'N64 Autoroute Toad',
        'GCN Désert Sec Sec', 'SNES Plaine Donut 3', 'N64 Autodrome royal', '3DS Forêt tropicale DK',
        'DS Stade Wario', 'GCN Royaume Sorbet', '3DS Piste musicale', 'N64 Vallée Yoshi',
        'DS Horloge Tic-Tac', '3DS Égout Piranha', 'Wii Volcan grondant', 'N64 Route Arc-en-ciel',
        'Wii Mine Wario', 'SNES Route Arc-en-ciel', 'Station Glagla', 'Circuit d\'Hyrule',
        '3DS Koopapolis', 'GBA Route Ruban', 'Métro Turbo', 'Big Blue',
	];
	
	let selectedTrack;

	$: if(selectedTrack) {
		console.log(selectedTrack.value);
	}
</script>

<style>
	.themed {
		max-width: 50%;
		--border: 1px solid grey;
		--borderRadius: 10px;
		--placeholderColor: grey;
	}
</style>

<main>
	<div class="themed">
		<Select items={tracks} bind:selectedValue={selectedTrack}></Select>
	</div>
</main>