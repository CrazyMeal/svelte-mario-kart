<script>
    const availableTracks = [
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

    let searchedTrack = '';
    let matchingTracks = [];

    $: if (searchedTrack !== '') {
        matchingTracks = [...availableTracks.filter(track => track.toLowerCase().indexOf(searchedTrack) > -1)];
    } else {
        matchingTracks = [];
    }

</script>

<input bind:value={searchedTrack}>

<ul>
{#each matchingTracks as track}
    <li>{ track }</li>
{/each}
</ul>

<style>
</style>