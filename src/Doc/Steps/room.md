# Room generation

## Room generation
The room generation is the first step of the generation process. It is the step where the room structure is created. The room is composed of a grid of cells. Each cell has 4 walls (North, East, South, West), a floor and ceiling. The algorithm starts by creating a grid of cells. It then chooses a random cell as the current cell and marks it as visited. The algorithm then chooses a random unvisited neighbor of the current cell and moves to it. The algorithm repeats this process until it reaches a cell that has no unvisited neighbors. When this happens, the algorithm backtracks to the previous cell and repeats the process until all cells have been visited. The result is an empty room with walls, floor and ceiling.

## Room generation parameters
The room generation algorithm has several parameters that can be adjusted to change the room structure. The parameters are as follows:

- **Width**: The width of the room in cells. default value is 2.5 meters.
- **Height**: The height of the room in cells. default value is 2.5 meters.
- **Max room size**: The maximum size of the room in cells. default value is room 40x40 cells.

## Openings placement

Openings are the doors and windows. To create one, you need to attach to the prefab a box collider and the ```Openings.cs``` script. This script will allow you to set the opening type (door or window) and the mean of opening (translation or rotation). You'll also need to link the opening's moving part, the center transform and the structure. 


<p align="center">
  <img src="../../Img/Window component.png" alt="Opening game object's components">
  <br>
  <em>Figure: Opening game object's components example </em>
</p>

Once the room layout is complete, exterior walls are procedurally replaced by walls containing doors and windows, to match the opening amount set by the user. These walls already contains windows, but doors are placed after the wall so the color can be chosen randomly.

To create a wall prefab, you can modify the base wall that has no openings and dig holes of the right size using the ProBuilder tool. You can then add either the windows to the prefab, or a door spawner with the ```WallDoor.cs``` script. 

These walls can then be added to the lists in the ```RoomGenerationData.asset``` scriptable object, located in ```Assets/Data``` so that they can be picked by the generation algorithm.

## Textures

After the walls and openings placement, textures are applied to each side of the room, to the floor, to the ceiling and to window frames.
 The textures are chosen from the 4 lists of textures in the ```RoomGenerationData.asset``` scriptable object, located in ```Assets/Data```.

To allow new textures to be used, you can create Unity's materials and add them to one of the lists. The materials must be set to the Standard shader and have a texture in the Albedo slot.
