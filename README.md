# Introduction
This repository hosts a SQLite database containing Formula 1 lap and race information for the seasons from 2014 to 2019
(i.e. the Formula 1 hybrid era with V6 engines). It was built mainly on the basis of the Ergast API (see
Attribution section) to be able to determine parameters for a race simulation (soon available on
https://github.com/TUMFTM/race-simulation). We are open for comments, suggestions and bug reports!

Contact person: [Alexander Heilmeier](mailto:alexander.heilmeier@tum.de).

# Attribution
Large parts of the database are based on the Ergast Motor Racing Developer API which can be found on
http://ergast.com/mrd/. The number of accidents and failures per driver and season is taken from the statistics sites of
https://www.racefans.net/, e.g. https://www.racefans.net/2018-f1-season/2018-f1-statistics/2018-f1-retirements-penalties/.

# Extensions and Modifications in comparison to the Ergast API
The following data was added in comparison to the Ergast database:
* FCY (full course yellow, i.e. VSC and SC) phases (per lap, race)
* driven tire compounds (per lap, race)
* number of planned laps (per race)
* accidents and failures (per driver, season)
* engine manufacturers (per race)
* comments (per race)
* track lengths (per race)

The following data was made directly accessible in comparison to the Ergast database:
* race time, gaps and intervals (per lap)
* number of driven laps (per race)
* finishing status (i.e. finished or disqualified or did not finish) (per race)

# License
The database can be used according to the terms of the Creative Commons Attribution-NonCommercial-ShareAlike 3.0
Unported (CC BY-NC-SA 3.0) license (see https://creativecommons.org/licenses/by-nc-sa/3.0/). This includes
non-commercial use, e.g. for research purposes.

# Accessing the database
The SQLite database can be viewed easily using online tools such as https://inloop.github.io/sqlite-viewer/.
In Python 3, it can be accessed using the sqlite3 package (https://docs.python.org/3.7/library/sqlite3.html).
A big advantage of the SQLite database is that all information is contained within a single file. Therefore, no
SQL server must be set up for usage.

# Content
The database contains the following tables:
* drivers
* fcyphases
* laps
* qualifyings
* races
* retirements
* starterfields

The data is available for the seasons 2014 - 2019.

Hint: Progress in the `fcyphases` and `laps` tables are calculated based on time, not on distance. Therefore, 0.8 means
that the phase started/ended at 80% of the final lap time, not at 80% of the track length (since this data is not
available).

Hint 2: The tire age for drivers starting in the top 10 is set to 2 laps by default (if they do not start on wet or
intermediate compound). This is made to take into account that these drivers have to start on the same tireset on which
they set their fastest lap time in Q2. The two laps are an approximation of the degradation relevant tire age (the real
tire age is three laps, at least, since there is usually one warm-up lap, one or two hot laps and one cool-down lap on
these tires).

Hint 3: All compounds are given in absolute compound names (A1 - A7) so that they can be compared over all seasons. The
following table contains which compound names of a season correspond to which absolute compound names.

| Season | A1 | A2 | A3 | A4 | A5 | A6 | A7 |
|---|---|---|---|---|---|---|---|
| 2014 | hard | medium | soft | supersoft | - | - | - |
| 2015 | hard | medium | soft | supersoft | - | - | - |
| 2016 | hard | medium | soft | supersoft | ultrasoft | - | - |
| 2017 | hard | medium | soft | supersoft | ultrasoft | - | - |
| 2018 | superhard | hard | medium | soft | supersoft | ultrasoft | hypersoft |
| 2019 | - | C1 | C2 | C3 | - | C4 | C5 |

### drivers table
| Field | Type | Key |
|---|---|---|
| id | integer | primary |
| carno | integer |  |
| initials | text |  |
| name | text |  |

### fcyphases table
| Field | Type | Key | Comment |
|---|---|---|---|
| id | integer | primary |  |
| race_id | integer | foreign |  |
| startracetime | real |  |  |
| endracetime | real |  |  |
| startraceprog | real |  | race progress of leader at phase start |
| endraceprog | real |  | race progress of leader at phase end |
| startlap | integer |  |  |
| endlap | integer |  |  |
| type | text |  | VSC or SC |

### laps table
| Field | Type | Key |
|---|---|---|
| race_id | integer | primary, foreign |
| lapno | integer | primary |
| position | integer | primary |
| driver_id | integer | foreign |
| laptime | real |  |
| racetime | real |  |
| gap | real | |
| interval | real |  |
| compound | text |  |
| tireage | integer |  |
| pitintime | text |  |
| pitstopduration | real |  |
| nextcompound | text |  |
| startlapprog_vsc | real |  |
| endlapprog_vsc | real |  |
| age_vsc | real |  |
| startlapprog_sc | real |  |
| endlapprog_sc | real |  |
| age_sc | real |  |

### qualifyings table
| Field | Type | Key |
|---|---|---|
| race_id | integer | primary, foreign |
| position | integer | primary |
| driver_id | integer | foreign |
| q1laptime | real |  |
| q2laptime | real |  |
| q3laptime | real |  |
| speedtrap | real |  |

### races table
| Field | Type | Key |
|---|---|---|
| id | integer | primary |
| date | text |  |
| season | integer |  |
| location | text |  |
| availablecompounds | text |  |
| comment | text |  |
| nolaps | integer |  |
| nolapsplanned | integer |  |
| tracklength | real |  |

### retirements table
| Field | Type | Key |
|---|---|---|
| season | integer | primary |
| driver_id | integer | primary, foreign |
| accidents | integer |  |
| failures | integer |  |

### starterfields table
| Field | Type | Key |
|---|---|---|
| race_id | integer | primary, foreign |
| driver_id | integer | primary, foreign |
| team | text |  |
| teamcolor | text |  |
| enginemanufacturer | text |  |
| gridposition | integer |  |
| status | text |  |
| resultposition | integer |  |
| completedlaps | integer |  |
| speedtrap | real |  |

# Related open-source repositories
* Lap time simulation: https://github.com/TUMFTM/laptime-simulation
* Lap-discrete race simulation: https://github.com/TUMFTM/race-simulation
* Time-discrete race simulator: https://github.com/heilmeiera/time-discrete-race-simulator
* Race track database: https://github.com/TUMFTM/racetrack-database
