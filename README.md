# Implementing Go in Excel

## Acknowledgements
Thank you to Jackson Stogel for aiding in the design of the algorithm. Thank you to Google for their excellent Google Sheets API for Python.

## Objective

The objective of this project is to implement a fully-functioning game of [Go](https://en.wikipedia.org/wiki/Go\_\(game\)) entirely in Google Sheets.

## Motivation

Here are some reasons why I did this:

    - Someone on Reddit mentioned they'd like to have it
    - It's Go in a format easily accessible to anyone with Google Drive (basically anyone these days), no apps needed.
    - It's excruciatingly funny and excruciating, but mainly excruciating

And why I did it the way I did:

    - Google Sheets has a Python API, so creation of sheets can be partially automated

## Sheets Description

Following is a description of each of the sheets in the document, and what they do.

### board
Displays the 19x19 playing board.

Each turn, cells are marked "b", "w", or left empty to indicate open space on the board. Cells will highlight red if they need to be deleted due to a shortage of liberties.

*Note:* Although this spreadsheet is 21x21, only the center 19x19 is used. The outer one pixel border is used for padding. This is a common theme for most of the spreadsheets.

### liberties
Calculates the number of empty spaces next to each stone on the board.

**This is different than the normal definition of liberties in the game!** It is defined here as "the number of empty spaces adjacent to a given board space". If a given board space has a stone, the corresponding liberties space will range from 0 to 4. If that space on the board is empty, the corresponding liberties space will read -1 instead.

### base\_ids
Enumerates each 19x19 space for flood fill algorithm.

This sheet enumerates each cell with an identifier from 1 to 361.

### black\_bfs
Identifies black [groups](https://en.wikipedia.org/wiki/Go\_\(game\)) on the board.

The algorithm is a standard bfs search, with depth states represented by each 21x21 block of data (also referred to as "state"). The first state is a copy of the base\_ids. Every state transition (transition from one data block to the next), we take each cell in the last 19x19 grid and set it equal to the minimum of all same-colored neighboring stones (including itself). This will result in an eventual convergent state where each group is marked by one ID (specifically, the smallest original base\_id of any stone in the group).

There are a total of 361 states, which is certainly overkill for the board (question for the reader: how many states are necessary to traverse any given group?). We use the last one as the convergent state.

###
Identifies white groups on the board.
