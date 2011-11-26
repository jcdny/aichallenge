Visualizing the state of your AI
================================

The `vis_overlay` branch adds support for visualizing your AI state in the game visualizer.

Installation
------------

* Clone the repository to your machine: `git clone git://github.com/j-h-a/aichallenge.git ./aichallenge`
* Switch to the `vis_overlay` branch: `git checkout vis_overlay`
* Initialize the submodules: `git submodule init; git submodule update`

This installs all you need to use the AI visualizer, it can be installed
alongside or instead of the tools directory from the main site.

Running Games
-------------

### Browser-based visualizer

Modify your game-execution script (outside the cloned repository) to use the cloned repository:
`python aichallenge/ants/playgame.py "$BOT0" "$BOT1" "$BOT2" "$BOT3" --map_file $MAP --log_dir game_logs --turns $TURNS --player_seed $SEED --verbose -e --turntime $TURNTIME`
*(The BOTn and MAP can point to any bots/maps in any directory, the main thing is to use the correct path for playgame.py)*

### Stand-alone real-time streaming visualizer

TODO: Please can someone tell me how to do this?

Visualizing AI State
--------------------

From your bot, in addition to the `o row col direction` commands you
output to control your ants' movement, you can also send the following
(case-sensitive) visualization commands:

* `v setLineWidth w`
* `v setLineColor r g b a`
* `v setFillColor r g b a`
* `v circle row col radius fill`
* `v rect row col width height fill`
* `v line row1 col1 row2 col2`
* `v arrow row1 col1 row2 col2`
* `v star row col inner_radius outer_radius points fill`
* `v tile row col`
* `v tileBorder row col subtile`
* `v tileSubTile row col subtile`
* `i row col rest-of-line-is-any-string-you-like`

### Notes:

* `setLineWidth` sets the with for all line-drawing commands. The line width `w` is in screen-units (not map-units).
* `setLineColour` and `setFillColour` set the colour values for all line-drawing and filled area drawing commands. Colours are specified as integers for `r`, `g`, and `b` (from 0-255) and the alpha value `a` is a float (from 0.0-1.0).
* 2D positions and dimensions (`row`, `col`, `radius`, `width`, `height`, etc.) are all floating point values in map-units. The row and column values are measured from the centre of each map square so if you want to draw shapes that line up with the map square boundaries (rather than centres) you have to add/subtract 0.5 from your numbers.
* The `fill` parameter used in some commands is a boolean and should be `true` or `false` (case-sensitive).
* `line` and `arrow` will draw the shortest path between the specified points, taking map-wrapping at the edges into account.
* `arrow` draws a line with an arrow-head at the end. The arrow-head length will be the smaller of two map-squares or half the line length.
* `star` draws a star centered at (`row`, `col`) with `points` points. `inner_radius` and `outer_radius` control the size of the star and points.
* `tile` draws a filled rectangle over the specified map-square. The same effect can be achieved with rect, but this is much easier if you want to exactly cover one square. Be sure to use a transparent fill-colour unless you want to completely block out the actual game square.
* `tileBorder` and `tileSubTile`: Imagine each tile (map-square) is divided into nine sub-tiles like a naughts-and-crosses board, the `subtile` parameter is a combination of Top-Middle-Bottom and Left-Middle-Right to define which of the nine sub-tiles you want to draw. It should be one of: `TL`, `TM`, `TR`, `ML`, `MM`, `MR`, `BL`, `BM`, `BR`. `tileSubTile` fills the specified sub-tile while `tileBorder` draws a line around the edge of the tile at the specified sub-tile location, or around the whole tile if `subtile` is `MM`.
* The `i` command adds map-information to a specific tile on the current turn. In the visualizer move your mouse over the tile to see the string you specified. If you specify this command more than once for the same row and column on any turn, the additional strings will be appended on a new line.


