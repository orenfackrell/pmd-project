# pmd-project

This is the start of my long time passion project to make a mystery dungeon style game. Now after spending the last three months learning to code mainly through the web development in the Odin Project and getting to grips using the cursor ide I am going to start with my making of this - currently my knowledge extends only to to webpage base JavaScript app / game. 

In the future I will try to learn how to move this to an actual game.

My research has lead my to this point to know what is needed for procedural dungeon generation

Each dungeon object will have the properties: 'name' - This will be determined by the level selected; 'floors' - Each level will have a preset number of floors; 'special terrain' - A non-floor tile which may be passed by certain types of Pokémon; 'mainRNG' - A randomized number.
    

There are some algorithms to generate each floor:

"Standard generator, 1/2 width" : An algorithm which makes best attempt to fill 1/2 of the gird with room tiles.

"One-room monster house" : An algorithm which makes all tiles room tiles aside from the outermost tiles being walls and the next ones in being 'special terrain'.

"Dense generator, corridors outside" : An algorithm which sets 5 >= M >= 3 & 5 >= N >= 3 place dummy rooms in the gird areas closest to the borders and rooms in all the other grid areas.

"Dense generator, corridors inside" : An algorithm which sets 5 >= M > 3 & 5 >= N >= 3 place rooms only in the gird areas closest to the borders and dummy rooms in all the other grid areas.

"Five-room generator, line arrangement" : An algorithm which sets M = 5 & 5 > N > 2 and fill only one row with 5 completed rooms.

"Five-room generator, cross arrangement" : An algorithm which sets 5 >= M >= 3 & 5 >= N >= 3 create 3 rooms a row (not the top or bottom row) and then create 1 room above and 1 room bellow the 2nd room in the row.

"Standard generator, full width" : An algorithm which makes best attempt to fill all of the gird with rooms.

"Middle-room layout": An algorithm which sets M = 3 & N = 3 and fill all gird areas and merge the middle column's rooms.

"Circular layout" : An algorithm which makes best attempt to fill all of the gird areas closest to the borders with room tiles.

"Standard generator, 3/4 width": An algorithm which makes best attempt to fill 3/4 of the gird with room tiles.


Each 'floor' object will have the properties randomly generated when they are loaded: 
'X': A random number between 0-56 will be used to determine the quantity of objects in the room on generation;
'roomD' (room density) : for as many grids being populated with rooms (call it R) generate a random number between -rooms.length < -2 & 2 < rooms.length ( if R is negative then |-R| is the exact number of rooms that will be generated, but if R is postive then a random number of rooms from R < rooms.length are generated) at least 2 rooms are always generated so if R being 1 or -1 is that same as it being 2 and -2 respectively;
'terrainD' : generate between X/2 and X 8x8 terrain spots; 
'tileD': generate between X/2 and X number of special tiles up to 56 tiles;
'groundItemD' : generate between X-2 and X+2 items on the floor;
'buriedItemD' : generate between X-2 and X+2 items inside the walls;
'deadEndD' : generate between X/2 and X number of dead ends will be attempted to be generated;

Each floor 56 x 32 tiles in size, with only the innermost 52 x 28 tiles being usable for - and the outside two tiles in each direction being reserved as borders. The outermost tiles a hard border made of wall which cannot be crossed ever, and the next tiles in being a 'soft' border, being made from walls of the special terrain for the level in the case of a "One-room monster house";
Begin level generating by placing all the border tiles and then making all the playable tiles 'wall' pieces;
Get floorStyle;
Separate the floor into random M number of columns and N number of rows according to the given ranges - unless stated other M & N range from 2 - 5;
According to the floorStyle populate the grid sections with a rectangle of room tiles placed randomly within the section, with a minimum width of 5 and minumum height of 4, prefering odd dimensions and assuring the ratio of these dimensions is always at least 2:3 (some rooms will be 1x1 dummy rooms depending on roomD value);
Each room will have a chance to connect to the neighboring room. Two random colinear points on their adjacent walls are selected and a line of room tiles is drawn between them passing through the selected points;
There is a 5% chance for neighboring rooms to be merged into one, meaning that there is a new larger room which covers the two adjacent rooms;
Use 'deadEndD' to create dead-ends: randomly select a wall tile and start drawing a line, at each step there is a chance to carry on, go left or go right or stop. Continue until either another room tile is one step away or the soft border of the map is reached;
Remake the soft and hard border of the floor and mark the entrances of all the rooms as 'entrance tiles' (including any dummy rooms);
If applicable generate a 'river' which is a random line of special terrain tiles which spans between 2 parallel walls (these can be the border walls);
According to the 'terrainD' value randomly select an amount of 8x8 terrain spots filled with a random amount (0-64) of special terrain tiles;
Once more remake the borders of the floor;
Check to see if the room is strongly connected (meaning that all rooms is reachable from anywhere), if not regenerate the floor;
If 10 invalid floors are generate default to "Five-room generator, line arrangement" floorStyle;
There is a random chance that a room on the floor will be designated as a 'shop room', there can only by one 'shop room' on any one floor;
If ('shop room') {
	replace room tiles with shop tiles;
	replace a random number of shop tile layers with room tiles ensure there is always at least a 3x3 grid of shop tiles remaining;
}
There is a random chance that a room on the floor will be designated as a 'monster house', there can only be one 'monster house' on any one floor;

Render the floor to the canvas element;