<script>
    import { leagueName, round } from '$lib/utils/helper';
  	import DataTable, { Head, Body, Row, Cell } from '@smui/data-table';
	import LinearProgress from '@smui/linear-progress';
    import { onMount } from 'svelte';
    import Standing from './Standing.svelte';
    import Head2Head from './Head2Head.svelte';
    import all_league_data from './ot.json'

    export let standingsData, usersData;

    // Least important to most important (i.e. the most important [usually wins] goes last)
    // Edit this to match your leagues settings
    const sortOrder = ["fptsAgainst", "divisionTies", "divisionWins", "fpts", "ties", "wins"];

    // Column order from left to right
    const columnOrder = [{name: "W", field: "wins"}, {name: "T", field: "ties"}, {name: "L", field: "losses"}, {name: "FPTS", field: "fpts"}, {name: "FPTS Against", field: "fptsAgainst"}, {name: "Streak", field: "streak"}]

    let loading = true;
    let preseason = false;
    let rosters, standings, year, users;
    onMount(async () => {
        const asyncStandingsData = await standingsData;
        if(!asyncStandingsData) {
            loading = false;
            preseason = true;
            return;
        }
        const {standingsInfo, yearData, rostersData} = asyncStandingsData;
        users = await usersData;
        rosters = rostersData;
        year = yearData;
        for(const standingKey in standingsInfo) {
            const roster = rosters[standingsInfo[standingKey].rosterID - 1];
            standingsInfo[standingKey].fpts = round(roster.settings.fpts + (roster.settings.fpts_decimal / 100));
            standingsInfo[standingKey].fptsAgainst = round(roster.settings.fpts_against + (roster.settings.fpts_against_decimal / 100));
	    standingsInfo[standingKey].streak = roster.metadata.streak;
        }

        let finalStandings = Object.keys(standingsInfo).map((key) => standingsInfo[key]);

        for(const sortType of sortOrder) {
            if(!finalStandings[0][sortType] && finalStandings[0][sortType] != 0) {
                continue;
            }
            finalStandings = [...finalStandings].sort((a,b) => b[sortType] - a[sortType]);
        }

        standings = finalStandings;
        loading = false;
    })

    let innerWidth;


    const h2h_table = all_league_data

    let h2h_data = h2h_table.data.Lathropolis.head_to_head;
    let total_w_l = h2h_table.data.Lathropolis.total_w_l;
    let w_l_keys = Object.keys(total_w_l);
    let w_l_entries = Object.entries(total_w_l);

    //headers.push('Total')

</script>

<svelte:window bind:innerWidth={innerWidth} />

<style>
    .loading {
        display: block;
        width: 85%;
        max-width: 500px;
        margin: 80px auto;
    }

    :global(.center) {
        text-align: center;
        vertical-align: middle;
    }

    :global(.wrappable) {
        white-space: normal;
        line-height: 1.2em;
    }

    h1 {
        font-size: 2.2em;
        line-height: 1.3em;
        margin: 1.5em 0 2em;
    }

    .h1Sub {
        margin: 0em 0 2em;
    }

    .standingsTable {
        max-width: 100%;
        margin: 0.5em 0 5em;
    }

    .h2hTable {
        max-width: 1400px;
        margin: 0 auto 2em;
    }

    :global(.rowHeader) {
        background-color: black;
        border-color: black;
    }
</style>

<h1>{year ?? ''} {leagueName} Standings</h1>

{#if loading}
    <!-- promise is pending -->
    <div class="loading">
        <p>Loading Standings...</p>
        <LinearProgress indeterminate />
    </div>
{:else if preseason}
<div class="loading">
    <p>Preseason, No Standings Yet</p>
</div>
{:else}
    <div class="standingsTable">
        <DataTable table$aria-label="League Standings" >
            <Head> <!-- Team name  -->
                <Row>
                    <Cell class="center">Team</Cell>
                    {#each columnOrder as column}
                        <Cell class="center wrappable">{column.name}</Cell>
                    {/each}
                </Row>
            </Head>
            <Body>
                <!-- 	Standing	 -->
                {#each standings as standing}
                    <Standing {columnOrder} {standing} user={users[rosters[standing.rosterID - 1].owner_id]} roster={rosters[standing.rosterID - 1]} />
                {/each}
            </Body>
        </DataTable>
    </div>
{/if}

<h1 style="margin: 0">All Time H2H</h1>
<span>Updated: 11/30/2022 (Post week 12 , 2022)</span>
<div class="h1Sub">(with playoffs. Red background means &lt; 30%, Green means &gt; 70%)</div>

<div class="h2hTable">
    <Head>
        <Row>
            <Cell class="center"></Cell>
            {#each h2h_data as owner}
                <Cell class="center">{owner[0]}</Cell>
            {/each}
        </Row>
    </Head>
    
    {#each h2h_data as owner}
    <Row>
        <Cell class="center rowHeader">
            {owner[0]}
        </Cell>
        {#each owner[1] as data}
            <Head2Head h2h_table={data[1]} />
        {/each}
    </Row>
    {/each}
</div>

<h1 style="margin: 0">Overall Regular Season W-L</h1>
<div class="standingsTable">
  <DataTable table$aria-label="League Standings" >
    <Head>
      <Row>
          {#each w_l_entries as [key, standing]}
              <Cell class="center">{key}</Cell>
          {/each}
      </Row>
  </Head>
    <Body>
      <!-- 	Standing	 -->
      {#each w_l_entries as [key, standing]}
          <Cell class="center">{standing.w} - {standing.l}</Cell>
      {/each}
  </Body>
</DataTable>
</div>
