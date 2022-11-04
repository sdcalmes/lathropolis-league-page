<script>
    import { leagueName, round } from '$lib/utils/helper';
	import { getTeamFromTeamManagers } from '$lib/utils/helperFunctions/universalFunctions';
  	import DataTable, { Head, Body, Row, Cell } from '@smui/data-table';
	import LinearProgress from '@smui/linear-progress';
    import { onMount } from 'svelte';
    import Standing from './Standing.svelte';
    import Head2Head from './Head2Head.svelte';

    export let standingsData, leagueTeamManagersData;

    // Least important to most important (i.e. the most important [usually wins] goes last)
    // Edit this to match your leagues settings
    const sortOrder = ["fptsAgainst", "divisionTies", "divisionWins", "fpts", "ties", "wins"];

    // Column order from left to right
    const columnOrder = [{name: "W", field: "wins"}, {name: "T", field: "ties"}, {name: "L", field: "losses"}, {name: "FPTS", field: "fpts"}, {name: "FPTS Against", field: "fptsAgainst"}, {name: "Streak", field: "streak"}]

    let loading = true;
    let preseason = false;
    let standings, year, leagueTeamManagers;
    onMount(async () => {
        const asyncStandingsData = await standingsData;
        if(!asyncStandingsData) {
            loading = false;
            preseason = true;
            return;
        }
        const {standingsInfo, yearData} = asyncStandingsData;
        leagueTeamManagers = await leagueTeamManagersData;
        year = yearData;

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


    const h2h_table =
    {
  "data": {
    "Lathropolis": {
      "year_data": {
        "2014": {
          "regular_standings": {
            "1": "Sam Calmes",
            "2": "Drew Davis",
            "3": "Andy Keal",
            "4": "justin alt",
            "5": "Victor Markus",
            "6": "Jake Folz",
            "7": "Joey Janz",
            "8": "James Stecker",
            "9": "Joe Keal",
            "10": "Peter Toninato",
            "11": "Matt Dallman",
            "12": "Luke Fowler"
          },
          "final_standings": {
            "1": "Sam Calmes",
            "2": "Drew Davis",
            "3": "Jake Folz",
            "4": "Victor Markus",
            "5": "Andy Keal",
            "6": "justin alt",
            "7": "Matt Dallman",
            "8": "Joey Janz",
            "9": "Peter Toninato",
            "10": "Joe Keal",
            "11": "James Stecker",
            "12": "Luke Fowler"
          }
        },
        "2015": {
          "regular_standings": {
            "1": "Sam Calmes",
            "2": "justin alt",
            "3": "Peter Toninato",
            "4": "Matt Dallman",
            "5": "Andy Keal",
            "6": "Drew Davis",
            "7": "Joe Keal",
            "8": "Luke Fowler",
            "9": "Joey Janz",
            "10": "Jake Folz",
            "11": "Victor Markus",
            "12": "James Stecker"
          },
          "final_standings": {
            "1": "Sam Calmes",
            "2": "Drew Davis",
            "3": "justin alt",
            "4": "Matt Dallman",
            "5": "Andy Keal",
            "6": "Peter Toninato",
            "7": "Luke Fowler",
            "8": "Victor Markus",
            "9": "James Stecker",
            "10": "Joey Janz",
            "11": "Joe Keal",
            "12": "Jake Folz"
          }
        },
        "2016": {
          "regular_standings": {
            "1": "Drew Davis",
            "2": "James Stecker",
            "3": "justin alt",
            "4": "Joey Janz",
            "5": "Sam Calmes",
            "6": "Matt Dallman",
            "7": "Jake Folz",
            "8": "Luke Fowler",
            "9": "Andy Keal",
            "10": "James Olson",
            "11": "Victor Markus",
            "12": "Joe Keal"
          },
          "final_standings": {
            "1": "James Stecker",
            "2": "Joey Janz",
            "3": "justin alt",
            "4": "Drew Davis",
            "5": "Sam Calmes",
            "6": "Matt Dallman",
            "7": "Andy Keal",
            "8": "Victor Markus",
            "9": "Joe Keal",
            "10": "Luke Fowler",
            "11": "Jake Folz",
            "12": "James Olson"
          }
        },
        "2017": {
          "regular_standings": {
            "1": "Luke Fowler",
            "2": "Drew Davis",
            "3": "Matt Dallman",
            "4": "Sam Calmes",
            "5": "Victor Markus",
            "6": "Jake Folz",
            "7": "Joe Keal",
            "8": "Joey Janz",
            "9": "Andy Keal",
            "10": "James Olson",
            "11": "James Stecker",
            "12": "justin alt"
          },
          "final_standings": {
            "1": "Luke Fowler",
            "2": "Drew Davis",
            "3": "Jake Folz",
            "4": "Victor Markus",
            "5": "Matt Dallman",
            "6": "Sam Calmes",
            "7": "Joey Janz",
            "8": "Andy Keal",
            "9": "James Stecker",
            "10": "Joe Keal",
            "11": "James Olson",
            "12": "justin alt"
          }
        },
        "2018": {
          "regular_standings": {
            "1": "Andy Keal",
            "2": "James Stecker",
            "3": "James Olson",
            "4": "Matt Dallman",
            "5": "Joe Keal",
            "6": "Drew Davis",
            "7": "Victor Markus",
            "8": "Joey Janz",
            "9": "Jake Folz",
            "10": "justin alt",
            "11": "Sam Calmes",
            "12": "Luke Fowler"
          },
          "final_standings": {
            "1": "James Stecker",
            "2": "Joe Keal",
            "3": "Andy Keal",
            "4": "Drew Davis",
            "5": "Matt Dallman",
            "6": "James Olson",
            "7": "justin alt",
            "8": "Victor Markus",
            "9": "Joey Janz",
            "10": "Jake Folz",
            "11": "Luke Fowler",
            "12": "Sam Calmes"
          }
        },
        "2019": {
          "regular_standings": {
            "1": "Drew Davis",
            "2": "Andy Keal",
            "3": "Victor Markus",
            "4": "Sam Calmes",
            "5": "Matt Dallman",
            "6": "Jake Folz",
            "7": "James Stecker",
            "8": "James Olson",
            "9": "Joey Janz",
            "10": "Joe Keal",
            "11": "justin alt",
            "12": "Luke Fowler"
          },
          "final_standings": {
            "1": "Drew Davis",
            "2": "Victor Markus",
            "3": "Sam Calmes",
            "4": "Andy Keal",
            "5": "Jake Folz",
            "6": "Matt Dallman",
            "7": "Joe Keal",
            "8": "James Olson",
            "9": "justin alt",
            "10": "Joey Janz",
            "11": "James Stecker",
            "12": "Luke Fowler"
          }
        },
        "2020": {
          "regular_standings": {
            "1": "Drew Davis",
            "2": "Joey Janz",
            "3": "Matt Dallman",
            "4": "Sam Calmes",
            "5": "justin alt",
            "6": "Luke Fowler",
            "7": "James Olson",
            "8": "Victor Markus",
            "9": "Joe Keal",
            "10": "James Stecker",
            "11": "Jake Folz",
            "12": "Andy Keal"
          },
          "final_standings": {
            "1": "Drew Davis",
            "2": "Matt Dallman",
            "3": "Sam Calmes",
            "4": "Joey Janz",
            "5": "Luke Fowler",
            "6": "justin alt",
            "7": "James Stecker",
            "8": "Joe Keal",
            "9": "Victor Markus",
            "10": "James Olson",
            "11": "Jake Folz",
            "12": "Andy Keal"
          }
        },
        "2021": {
          "regular_standings": {
            "1": "Luke Fowler",
            "2": "Victor Markus",
            "3": "James Olson",
            "4": "Joe Keal",
            "5": "Sam Calmes",
            "6": "Andy Keal",
            "7": "Drew Davis",
            "8": "Joey Janz",
            "9": "justin alt",
            "10": "Jake Folz",
            "11": "James Stecker",
            "12": "Matt Dallman"
          },
          "final_standings": {
            "1": "James Olson",
            "2": "Sam Calmes",
            "3": "Luke Fowler",
            "4": "Victor Markus",
            "5": "Andy Keal",
            "6": "Joe Keal",
            "7": "justin alt",
            "8": "Drew Davis",
            "9": "Joey Janz",
            "10": "James Stecker",
            "11": "Matt Dallman",
            "12": "Jake Folz"
          }
        },
        "2022": {
          "regular_standings": {
            "1": "Jake Folz",
            "2": "Andy Keal",
            "3": "Matt Dallman",
            "4": "Sam Calmes",
            "5": "James Stecker",
            "6": "Joey Janz",
            "7": "justin alt",
            "8": "Joe Keal",
            "9": "Victor Markus",
            "10": "Luke Fowler",
            "11": "James Olson",
            "12": "Drew Davis"
          },
          "final_standings": {
            
          }
        }
      },
      "total_w_l": {
        "Drew Davis": {
          "w": 93,
          "l": 68
        },
        "Victor Markus": {
          "w": 80,
          "l": 81
        },
        "justin alt": {
          "w": 80,
          "l": 81
        },
        "Joey Janz": {
          "w": 84,
          "l": 77
        },
        "Jake Folz": {
          "w": 75,
          "l": 86
        },
        "Matt Dallman": {
          "w": 80,
          "l": 81
        },
        "Sam Calmes": {
          "w": 96,
          "l": 65
        },
        "James Stecker": {
          "w": 74,
          "l": 87
        },
        "Andy Keal": {
          "w": 85,
          "l": 76
        },
        "Peter Toninato": {
          "w": 12,
          "l": 14
        },
        "Joe Keal": {
          "w": 71,
          "l": 90
        },
        "Luke Fowler": {
          "w": 67,
          "l": 94
        },
        "James Olson": {
          "w": 69,
          "l": 66
        }
      },
      "average_regular_standing": {
        "Drew Davis": 4.22,
        "Sam Calmes": 4.33,
        "Andy Keal": 5.44,
        "Matt Dallman": 5.67,
        "Peter Toninato": 6.5,
        "Victor Markus": 6.78,
        "Joey Janz": 6.78,
        "justin alt": 7.0,
        "Jake Folz": 7.33,
        "James Olson": 7.43,
        "James Stecker": 7.56,
        "Luke Fowler": 7.78,
        "Joe Keal": 7.89
      },
      "average_final_standing": {
        "Drew Davis": 3.0,
        "Sam Calmes": 4.12,
        "Matt Dallman": 5.75,
        "Victor Markus": 5.88,
        "Andy Keal": 6.12,
        "justin alt": 6.62,
        "Joey Janz": 7.38,
        "James Stecker": 7.38,
        "Peter Toninato": 7.5,
        "Luke Fowler": 7.62,
        "Joe Keal": 7.88,
        "James Olson": 8.0,
        "Jake Folz": 8.38
      },
      "average_standing_difference": {
        "Drew Davis": 1.22,
        "Victor Markus": 0.9,
        "justin alt": 0.38,
        "Sam Calmes": 0.21,
        "James Stecker": 0.18,
        "Luke Fowler": 0.16,
        "Joe Keal": 0.01,
        "Matt Dallman": -0.08,
        "James Olson": -0.57,
        "Joey Janz": -0.6,
        "Andy Keal": -0.68,
        "Peter Toninato": -1.0,
        "Jake Folz": -1.05
      },
      "head_to_head": [
        [
          "Andy Keal",
          [
            [
              "Andy Keal",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "Drew Davis",
              {
                "w": 7,
                "l": 6
              }
            ],
            [
              "Jake Folz",
              {
                "w": 4,
                "l": 7
              }
            ],
            [
              "James Olson",
              {
                "w": 6,
                "l": 3
              }
            ],
            [
              "James Stecker",
              {
                "w": 9,
                "l": 4
              }
            ],
            [
              "Joe Keal",
              {
                "w": 9,
                "l": 3
              }
            ],
            [
              "Joey Janz",
              {
                "w": 6,
                "l": 5
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 6,
                "l": 3
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 6,
                "l": 9
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 4,
                "l": 0
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 5,
                "l": 6
              }
            ],
            [
              "Victor Markus",
              {
                "w": 3,
                "l": 9
              }
            ],
            [
              "justin alt",
              {
                "w": 5,
                "l": 5
              }
            ]
          ]
        ],
        [
          "Drew Davis",
          [
            [
              "Andy Keal",
              {
                "w": 6,
                "l": 7
              }
            ],
            [
              "Drew Davis",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "Jake Folz",
              {
                "w": 7,
                "l": 6
              }
            ],
            [
              "James Olson",
              {
                "w": 7,
                "l": 1
              }
            ],
            [
              "James Stecker",
              {
                "w": 4,
                "l": 6
              }
            ],
            [
              "Joe Keal",
              {
                "w": 5,
                "l": 6
              }
            ],
            [
              "Joey Janz",
              {
                "w": 5,
                "l": 5
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 7,
                "l": 3
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 5,
                "l": 5
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 3,
                "l": 0
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 5,
                "l": 8
              }
            ],
            [
              "Victor Markus",
              {
                "w": 7,
                "l": 5
              }
            ],
            [
              "justin alt",
              {
                "w": 10,
                "l": 5
              }
            ]
          ]
        ],
        [
          "Jake Folz",
          [
            [
              "Andy Keal",
              {
                "w": 7,
                "l": 4
              }
            ],
            [
              "Drew Davis",
              {
                "w": 6,
                "l": 7
              }
            ],
            [
              "Jake Folz",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "James Olson",
              {
                "w": 7,
                "l": 4
              }
            ],
            [
              "James Stecker",
              {
                "w": 5,
                "l": 6
              }
            ],
            [
              "Joe Keal",
              {
                "w": 3,
                "l": 7
              }
            ],
            [
              "Joey Janz",
              {
                "w": 4,
                "l": 7
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 11,
                "l": 4
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 7,
                "l": 7
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 2,
                "l": 2
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 3,
                "l": 8
              }
            ],
            [
              "Victor Markus",
              {
                "w": 3,
                "l": 9
              }
            ],
            [
              "justin alt",
              {
                "w": 5,
                "l": 4
              }
            ]
          ]
        ],
        [
          "James Olson",
          [
            [
              "Andy Keal",
              {
                "w": 3,
                "l": 6
              }
            ],
            [
              "Drew Davis",
              {
                "w": 1,
                "l": 7
              }
            ],
            [
              "Jake Folz",
              {
                "w": 4,
                "l": 7
              }
            ],
            [
              "James Olson",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "James Stecker",
              {
                "w": 5,
                "l": 5
              }
            ],
            [
              "Joe Keal",
              {
                "w": 2,
                "l": 8
              }
            ],
            [
              "Joey Janz",
              {
                "w": 7,
                "l": 2
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 4,
                "l": 5
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 5,
                "l": 3
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 4,
                "l": 4
              }
            ],
            [
              "Victor Markus",
              {
                "w": 5,
                "l": 3
              }
            ],
            [
              "justin alt",
              {
                "w": 6,
                "l": 5
              }
            ]
          ]
        ],
        [
          "James Stecker",
          [
            [
              "Andy Keal",
              {
                "w": 4,
                "l": 9
              }
            ],
            [
              "Drew Davis",
              {
                "w": 6,
                "l": 4
              }
            ],
            [
              "Jake Folz",
              {
                "w": 6,
                "l": 5
              }
            ],
            [
              "James Olson",
              {
                "w": 5,
                "l": 5
              }
            ],
            [
              "James Stecker",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "Joe Keal",
              {
                "w": 11,
                "l": 3
              }
            ],
            [
              "Joey Janz",
              {
                "w": 6,
                "l": 5
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 4,
                "l": 7
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 4,
                "l": 6
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 1,
                "l": 1
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 3,
                "l": 9
              }
            ],
            [
              "Victor Markus",
              {
                "w": 7,
                "l": 4
              }
            ],
            [
              "justin alt",
              {
                "w": 6,
                "l": 9
              }
            ]
          ]
        ],
        [
          "Joe Keal",
          [
            [
              "Andy Keal",
              {
                "w": 3,
                "l": 9
              }
            ],
            [
              "Drew Davis",
              {
                "w": 6,
                "l": 5
              }
            ],
            [
              "Jake Folz",
              {
                "w": 7,
                "l": 3
              }
            ],
            [
              "James Olson",
              {
                "w": 8,
                "l": 2
              }
            ],
            [
              "James Stecker",
              {
                "w": 3,
                "l": 11
              }
            ],
            [
              "Joe Keal",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "Joey Janz",
              {
                "w": 4,
                "l": 10
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 9,
                "l": 4
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 6,
                "l": 7
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 1,
                "l": 3
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 3,
                "l": 6
              }
            ],
            [
              "Victor Markus",
              {
                "w": 5,
                "l": 8
              }
            ],
            [
              "justin alt",
              {
                "w": 6,
                "l": 4
              }
            ]
          ]
        ],
        [
          "Joey Janz",
          [
            [
              "Andy Keal",
              {
                "w": 5,
                "l": 6
              }
            ],
            [
              "Drew Davis",
              {
                "w": 5,
                "l": 5
              }
            ],
            [
              "Jake Folz",
              {
                "w": 7,
                "l": 4
              }
            ],
            [
              "James Olson",
              {
                "w": 2,
                "l": 7
              }
            ],
            [
              "James Stecker",
              {
                "w": 5,
                "l": 6
              }
            ],
            [
              "Joe Keal",
              {
                "w": 10,
                "l": 4
              }
            ],
            [
              "Joey Janz",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 7,
                "l": 5
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 4,
                "l": 8
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 2,
                "l": 1
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 7,
                "l": 5
              }
            ],
            [
              "Victor Markus",
              {
                "w": 10,
                "l": 5
              }
            ],
            [
              "justin alt",
              {
                "w": 6,
                "l": 6
              }
            ]
          ]
        ],
        [
          "Luke Fowler",
          [
            [
              "Andy Keal",
              {
                "w": 3,
                "l": 6
              }
            ],
            [
              "Drew Davis",
              {
                "w": 3,
                "l": 7
              }
            ],
            [
              "Jake Folz",
              {
                "w": 4,
                "l": 11
              }
            ],
            [
              "James Olson",
              {
                "w": 5,
                "l": 4
              }
            ],
            [
              "James Stecker",
              {
                "w": 7,
                "l": 4
              }
            ],
            [
              "Joe Keal",
              {
                "w": 4,
                "l": 9
              }
            ],
            [
              "Joey Janz",
              {
                "w": 5,
                "l": 7
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 3,
                "l": 8
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 0,
                "l": 2
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 6,
                "l": 6
              }
            ],
            [
              "Victor Markus",
              {
                "w": 8,
                "l": 4
              }
            ],
            [
              "justin alt",
              {
                "w": 6,
                "l": 8
              }
            ]
          ]
        ],
        [
          "Matt Dallman",
          [
            [
              "Andy Keal",
              {
                "w": 9,
                "l": 6
              }
            ],
            [
              "Drew Davis",
              {
                "w": 5,
                "l": 5
              }
            ],
            [
              "Jake Folz",
              {
                "w": 7,
                "l": 7
              }
            ],
            [
              "James Olson",
              {
                "w": 3,
                "l": 5
              }
            ],
            [
              "James Stecker",
              {
                "w": 6,
                "l": 4
              }
            ],
            [
              "Joe Keal",
              {
                "w": 7,
                "l": 6
              }
            ],
            [
              "Joey Janz",
              {
                "w": 8,
                "l": 4
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 8,
                "l": 3
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 1,
                "l": 1
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 2,
                "l": 14
              }
            ],
            [
              "Victor Markus",
              {
                "w": 5,
                "l": 5
              }
            ],
            [
              "justin alt",
              {
                "w": 4,
                "l": 7
              }
            ]
          ]
        ],
        [
          "Peter Toninato",
          [
            [
              "Andy Keal",
              {
                "w": 0,
                "l": 4
              }
            ],
            [
              "Drew Davis",
              {
                "w": 0,
                "l": 3
              }
            ],
            [
              "Jake Folz",
              {
                "w": 2,
                "l": 2
              }
            ],
            [
              "James Stecker",
              {
                "w": 1,
                "l": 1
              }
            ],
            [
              "Joe Keal",
              {
                "w": 3,
                "l": 1
              }
            ],
            [
              "Joey Janz",
              {
                "w": 1,
                "l": 2
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 2,
                "l": 0
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 1,
                "l": 1
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 2,
                "l": 2
              }
            ],
            [
              "Victor Markus",
              {
                "w": 2,
                "l": 0
              }
            ],
            [
              "justin alt",
              {
                "w": 0,
                "l": 2
              }
            ]
          ]
        ],
        [
          "Sam Calmes",
          [
            [
              "Andy Keal",
              {
                "w": 6,
                "l": 5
              }
            ],
            [
              "Drew Davis",
              {
                "w": 8,
                "l": 5
              }
            ],
            [
              "Jake Folz",
              {
                "w": 8,
                "l": 3
              }
            ],
            [
              "James Olson",
              {
                "w": 4,
                "l": 4
              }
            ],
            [
              "James Stecker",
              {
                "w": 9,
                "l": 3
              }
            ],
            [
              "Joe Keal",
              {
                "w": 6,
                "l": 3
              }
            ],
            [
              "Joey Janz",
              {
                "w": 5,
                "l": 7
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 6,
                "l": 6
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 14,
                "l": 2
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 2,
                "l": 2
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "Victor Markus",
              {
                "w": 7,
                "l": 6
              }
            ],
            [
              "justin alt",
              {
                "w": 6,
                "l": 4
              }
            ]
          ]
        ],
        [
          "Victor Markus",
          [
            [
              "Andy Keal",
              {
                "w": 9,
                "l": 3
              }
            ],
            [
              "Drew Davis",
              {
                "w": 5,
                "l": 7
              }
            ],
            [
              "Jake Folz",
              {
                "w": 9,
                "l": 3
              }
            ],
            [
              "James Olson",
              {
                "w": 3,
                "l": 5
              }
            ],
            [
              "James Stecker",
              {
                "w": 4,
                "l": 7
              }
            ],
            [
              "Joe Keal",
              {
                "w": 8,
                "l": 5
              }
            ],
            [
              "Joey Janz",
              {
                "w": 5,
                "l": 10
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 4,
                "l": 8
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 5,
                "l": 5
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 0,
                "l": 2
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 6,
                "l": 7
              }
            ],
            [
              "Victor Markus",
              {
                "w": 0,
                "l": 0
              }
            ],
            [
              "justin alt",
              {
                "w": 6,
                "l": 6
              }
            ]
          ]
        ],
        [
          "justin alt",
          [
            [
              "Andy Keal",
              {
                "w": 5,
                "l": 5
              }
            ],
            [
              "Drew Davis",
              {
                "w": 5,
                "l": 10
              }
            ],
            [
              "Jake Folz",
              {
                "w": 4,
                "l": 5
              }
            ],
            [
              "James Olson",
              {
                "w": 5,
                "l": 6
              }
            ],
            [
              "James Stecker",
              {
                "w": 9,
                "l": 6
              }
            ],
            [
              "Joe Keal",
              {
                "w": 4,
                "l": 6
              }
            ],
            [
              "Joey Janz",
              {
                "w": 6,
                "l": 6
              }
            ],
            [
              "Luke Fowler",
              {
                "w": 8,
                "l": 6
              }
            ],
            [
              "Matt Dallman",
              {
                "w": 7,
                "l": 4
              }
            ],
            [
              "Peter Toninato",
              {
                "w": 2,
                "l": 0
              }
            ],
            [
              "Sam Calmes",
              {
                "w": 4,
                "l": 6
              }
            ],
            [
              "Victor Markus",
              {
                "w": 6,
                "l": 6
              }
            ],
            [
              "justin alt",
              {
                "w": 0,
                "l": 0
              }
            ]
          ]
        ]
      ]
    }
  }
}

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
                    <Standing {columnOrder} {standing} {leagueTeamManagers} team={getTeamFromTeamManagers(leagueTeamManagers, standing.rosterID)} />
                {/each}
            </Body>
        </DataTable>
    </div>
{/if}

<h1 style="margin: 0">All Time H2H</h1>
<span>Updated: 11/3/2022 (Post week 8 , 2022)</span>
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
