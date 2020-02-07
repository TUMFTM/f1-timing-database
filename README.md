# Introduction
This repository hosts a SQLite database containing Formula 1 lap and race information for the seasons 2014 - 2019
(i.e. the Formula 1 hybrid era). It was built mainly on the basis of the Ergast API (see Attribution) to be able to
determine parameters for a race simulation (soon available on https://github.com/TUMFTM/race_strategy_tools). We are
open for comments, suggestions and bug reports!

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

The following data was made directly accessible in comparison to the Ergast database:
* gaps and intervals (per lap)
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

### drivers table
| Field | Type | Key |
|---|---|---|
| id | integer | primary |
| carno | integer |  |
| initials | text |  |
| name | text |  |

### fcyphases table
| Field | Type | Key |
|---|---|---|
| race_id | integer | primary, foreign |
| startlap | integer | primary |
| endlap | integer |  |
| type | text |  |

### laps table
| Field | Type | Key |
|---|---|---|
| race_id | integer | primary, foreign |
| lapno | integer | primary |
| position | integer | primary |
| driver_id | integer | foreign |
| laptime | real |  |
| gap | real | |
| interval | real |  |
| compound | text |  |
| tireage | integer |  |
| pitintime | text |  |
| pitstoptime | real |  |
| nextcompound | text |  |
| fcyphasetype | text |  |
| fcyphaseage | integer |  |

### qualifyings table
| Field | Type | Key |
|---|---|---|
| race_id | integer | primary, foreign |
| position | integer | primary |
| driver_id | integer | foreign |
| q1time | real |  |
| q2time | real |  |
| q3time | real |  |

### races table
| Field | Type | Key |
|---|---|---|
| id | integer | primary |
| date | text |  |
| location | text |  |
| availablecompounds | text |  |
| comment | text |  |
| nolaps | integer |  |
| nolapsplanned | integer |  |

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
| enginemanufacturer | text |  |
| gridposition | integer |  |
| status | text |  |
