# lastfm-export
### Export your Last.fm tracks and import them into a MySQL/MariaDb or Sqlite database

This script imports your Last.fm listening history inside a MySQL/MariaDb or Sqlite database.

The first run might take quite some time depending on your listening history.
But next runs will be much faster as only missing plays are imported into the database.

## 2020-05-29

List of changes:

**database**
- one json_track table to keep the raw json from Last.fm
    - much easier to reimport everything from scratch without querying Last.fm.
- one play table instead of the former 4 tables (artist, album, track, play)
    - getting rid of normalisation but fixing artist names is much easier with a single table.
- using liquibase to create the database
- choose between MySQL/MariaDb and Sqlite

- for **MySQL/MariDb**:
    - update the liquibase.properties in the liquibase folder
    - execute the init-user-lastfm.sql script
    - update the database with liquibase:
    ```bash
    liquibase --logLevel DEBUG --changeLogFile db-changelog.xml update
    ```
- for **Sqlite**:
    - create the database by running createSqliteDb.sh in the liquibase folder.
    - move the created lastfm.sqlite file to the parent directory.
- no more views for number of days and year tops

**scripts**
- a yaml config file: copy the example file in ~/.config/lastfm-export/lastfm-export.yml
- export.py gets the new tracks from Last.fm and inserts them into json_track.
- import.py imports the missing tracks into the play table.
 
**plots**

Instead of using Python notebooks and offline Plotly (see lastfm-notebooks),
using matplotlib to plot and save images locally.

- plot_top.py plots:
    - a last number of days 7, 30, 90, 180, 365 or overall artist/albums/tracks top
    - a given year artist/albums/tracks top
- plot_counts.py plots number of plays per month per year.
- plot_artist_counts plots artist play counts for:
    - all time
    - for a year
    - TODO: for a month
- TODO: plot_albums_counts

A Telegram bot in the bot module to get an artist/albums/tracks top
- by last number of days: 7, 30, 90, 180, 365 or overall
- by year

Copyright (c) 2015-2020, Nicolas Meier
