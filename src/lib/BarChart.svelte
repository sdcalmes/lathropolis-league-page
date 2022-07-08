<script>
    import Button, { Group, Label } from '@smui/button';
	import Bar from './Bar.svelte';

    export let graphs, leagueTeamManagers, curGraph = 0;

    const colors = [
        "--barChartOne",
        "--barChartTwo",
        "--barChartThree",
        "--barChartFour",
        "--barChartFive",
        "--barChartSix",
    ];

    // note that due to changig to horizontal, yMin and yMax are now used as xMin and xMax
    $: xMin = graphs[curGraph].secondStats.length > 0 ? graphs[curGraph].xMin/3 : graphs[curGraph].xMin;
    $: xMax = graphs[curGraph].xMax;
    $: stats = graphs[curGraph].stats;
    $: secondStats = graphs[curGraph].secondStats;
    $: managerIDs = graphs[curGraph].managerIDs;
    $: rosterIDs = graphs[curGraph].rosterIDs;
    $: labels = graphs[curGraph].labels;
    $: header = graphs[curGraph].header;
    $: year = graphs[curGraph].year;
</script>

<style>
    .chartWrapper {
		background-color: var(--fff);
        padding: 1em 0 0.5em;
        margin: 0 auto;
        max-width: 950px;
        box-shadow: 0px 3px 3px -2px var(--boxShadowOne), 0px 3px 4px 0px var(--boxShadowTwo), 0px 1px 8px 0px var(--boxShadowThree);
    }

    .barChart {
        display: block;
        position: relative;
        width: 100%;
        height: 100%;
    }

    h6 {
        font-weight: 400;
        width: 100%;
        text-align: center;
        margin: 0 0 1em;
    }

    .buttonHolderG {
        text-align: center;
        margin: 1em 0 2em;
    }

    /* Start button resizing */
    @media (max-width: 1000px) {
        .chartWrapper {
            max-width: 95%;
        }
    }
    @media (max-width: 850px) {
        .chartWrapper {
            max-width: 100%;
        }
    }

    @media (max-width: 525px) {
        :global(.buttonHolderG .selectionButtons) {
            font-size: 0.6em;
            height: 32px;
        }
    }

    @media (max-width: 505px) {
        :global(.buttonHolderG .selectionButtons) {
            height: 26px;
        }
    }

    @media (max-width: 405px) {
        :global(.buttonHolderG .selectionButtons) {
            font-size: 0.5em;
            padding: 0 6px;
        }
    }

    @media (max-width: 260px) {
        :global(.buttonHolderG .selectionButtons) {
            font-size: 0.4em;
            padding: 0 2px;
            height: 24px;
            min-width: 40px;
        }
    }

    /* End button resizing */
</style>

<h6>{header}</h6>
<div class="chartWrapper">
    <div class="barChart" >
        <div class="barChartInner" style="width: {width * .85}px; height: {width * .7 * .66}px" bind:this={el}>
            <!-- x Axis label and intervals -->
            {#each stats as stat, ix}
                <div class="bar{secondStats.length == 0  ? '' : ' opacity'}" style="background-color: {colors[(rosterIDs[ix]-1) % 12]}; width: {chartWidthInterval * 0.8}px; left: {chartWidthInterval * ix + (chartWidthInterval / 2)}px; height: {(stat - yMin) / (yMax - yMin == 0 ? 1 : (yMax - yMin)) * 100}%;">
                    <span class="barLabel" style="font-size: {barLFont}em;">{stat}{labels.stat}</span>
                </div>
            {/each}
            {#each secondStats as stat, ix}
                <div class="bar" style="background-color: {colors[(rosterIDs[ix]-1) % 12]}; width: {chartWidthInterval * 0.8}px; left: {chartWidthInterval * ix + (chartWidthInterval / 2)}px; height: {(stat - yMin) / (yMax - yMin == 0 ? 1 : (yMax - yMin)) * 100}%;" />
            {/each}
        </div>

        <!-- y Axis label and intervals -->
        <div class="yAxis">
            <div class="label yLabel" style="height: {chartHeight}px; font-size: {lFont}em;">{labels.y}</div>
            <div class="yIntervals" style="right: {chartWidth + 65}px; height: {chartHeight}px;">
                {#each yScales as yScale, ix}
                    <div class="label yInterval" style="bottom: {chartHeightInterval * ix}px; font-size: {lFont}em;">{yScale}</div>
                {/each}
            </div>
        </div>

        <!-- x Axis label and intervals -->
        <div class="xAxis">
            <div class="label xLabel" style="width: {chartWidth}px; font-size: {lFont}em;">{labels.x}</div>
            <div class="xIntervals" style="width: {chartWidth}px; top: {chartHeight + 6}px;">
                {#each managers as manager, ix}
                    <div class="xInterval" style="left: {chartWidthInterval * (ix + 0.5)}px; font-size: {xIFont}em; height: {xIHeight}px;">{manager.display_name}</div>
                {/each}
            </div>
        </div>
    </div>
</div>

{#if graphs.length > 1}
    <div class="buttonHolderG">
        <Group variant="outlined">
            {#each graphs as graph, ix}
                {#if ix < 4}
                    <Button class="selectionButtons" on:click={() => curGraph = ix} variant="{curGraph == ix ? "raised" : "outlined"}">
                        <Label>{graph.short}</Label>
                    </Button>
                {/if}
            {/each}
        </Group>
        <br />
        <Group variant="outlined">
            {#each graphs as graph, ix}
                {#if ix > 3}
                    <Button class="selectionButtons" on:click={() => curGraph = ix} variant="{curGraph == ix ? "raised" : "outlined"}">
                        <Label>{graph.short}</Label>
                    </Button>
                {/if}
            {/each}
        </Group>
    </div>
{/if}