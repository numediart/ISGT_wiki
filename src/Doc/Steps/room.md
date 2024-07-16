# Room generation

## Room generation
The room generation is the first step of the generation process. It is the step where the room structure is created. The room is composed of a grid of cells. Each cell has 4 walls (North, East, South, West), a floor and ceiling. The algorithm starts by creating a grid of cells. It then chooses a random cell as the current cell and marks it as visited. The algorithm then chooses a random unvisited neighbor of the current cell and moves to it. The algorithm repeats this process until it reaches a cell that has no unvisited neighbors. When this happens, the algorithm backtracks to the previous cell and repeats the process until all cells have been visited. The result is an empty room with walls, floor and ceiling.

## Room generation parameters
The room generation algorithm has several parameters that can be adjusted to change the room structure. The parameters are as follows:

- **Width**: The width of the room in cells. default value is 2.5 meters.
- **Height**: The height of the room in cells. default value is 2.5 meters.
- **Max room size**: The maximum size of the room in cells. default value is room 40x40 cells.

## Room generation example
The following example shows how to generate a room with the default parameters:

```Csharp
using ClassicRoom;
using UnityEngine;

public class RoomGenerationExample : MonoBehaviour
{
   
    [SerializeField] private RoomGenerationData roomGenerationData;
    void Start()
    {
        ClassicRoom room = gameObject.AddComponent<ClassicRoom>();

        room.InitRoom(roomGenerationData);//generate room with defined parameters
    }
}
```


