<script>
    import { sidebets } from '$lib/utils/helper';

    console.log(sidebets)

</script>

<style>
    #sidebets {
        border-collapse: collapse;
        width: 100%
    }

    #sidebets td, #sidebets th {
        border: 1px solid rgb(29, 29, 29);
        padding: 8px;
    }

    #sidebets tr:nth-child(even){
        background-color: #f2f2f21a;
    }

    #sidebets tr:hover {
        background-color: rgb(126, 126, 126);
    }

    #sidebets th {
        padding-top: 12px;
        padding-bottom: 12px;
        text-align: left;
        background-color: #0d221a;
        color: white;
    }

    .ownerHeader, .winnerHeader {
        width: 9%;
    }

    .rewardsHeader {
        width: 20%;
    }

</style>

<div class="sidebetContainer">
    <div>
        {#each sidebets as sidebetYear}
            <h1>{sidebetYear.year}</h1>
            <table id="sidebets">
                <tr>
                    <th class="ownerHeader">Owner A</th>
                    <th class="ownerHeader">Owner B</th>
                    <th class="rewardsHeader">Reward/Consequence</th>
                    <th class="detailsHeader">Details</th>
                    <th class="winnerHeader">Winner</th>
                </tr>
                {#each sidebetYear.sidebets as bet}
                    <tr>
                        <td>{bet.owner1}</td>
                        <td>{bet.owner2}</td>
                        <td>{bet.reward}</td>
                        <td>{bet.details}</td>
                        <td>{bet.winner}</td>
                    </tr>
                {/each}
            </table>
        {/each}
        </div>
</div>