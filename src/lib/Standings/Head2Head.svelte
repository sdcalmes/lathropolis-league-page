<script>
    import { Row, Cell } from '@smui/data-table';

    export let h2h_table;

    let pct = h2h_table.w / (h2h_table.w + h2h_table.l)
    console.log(getColor(pct))

    function getColor(value){
        //value from 0 to 1
        var hue=((value)*120).toString(10);
        if (value > .7 || value < .3)
            hue = 'NaN'
        return ["hsl(",hue,",100%,50%)"].join("");
    }


</script>

<style>
    :global(.h2hCellHigh) {
        font-weight: bolder;
        font-style: italic;
        background-color:green;
        color: white;
    }

    :global(.h2hCellLow) {
        font-weight:lighter;
        font-style: italic;
        background-color: red;
    }

</style>
<Cell class="center {pct > .70 ? 'h2hCellHigh' : pct < .30 ? 'h2hCellLow' : ''}" style="color: {getColor(pct)};">
    {h2h_table.w} - {h2h_table.l}
</Cell>
