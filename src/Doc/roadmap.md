# Roadmap

ISTG is still in development, here are some ideas for future improvements :

### Better props placement

The props placement is currently done using a quad tree, which is a fast way to avoid overlapping. However, the placement could be improved by adding more rules for certain types of props. But having to write specific rules for each type of prop can be tedious. A better way would be to use an AI model to predict the best placement for each prop. 

### Different room types

Currently, the room is generated with a single type of room in mind, which is a common living room. Adding more types of rooms, such as bedrooms, kitchens, or even offices or shed would be a great improvement. This would require adding new types of furniture, of textures and new rules for the props placement.

Other shapes of room than rectangles could also be generated, such as L-shaped rooms, or rooms with multiple floors.

### Improved visual quality

To ensure that the model trained with the data behaves well in real life, the quality of the images must be as realistic as possible. This can be achieved by adding more detailed textures, more complexe props and to implement shaders to improve the lighting.

It would also be interesting to add post-processing effects to the camera to make the images more realistic. 

Generating the rooms next to each other would also help by allowing to see other rooms through the windows, instead of just the skybox.


### Other objects detection

Currently, the software only generates data about doors and windows because its initial purpose was to train an AI for a drone to detect these objects. However, the software could be used to generate data about other objects, such as furniture, plants, or even people. This would require adding new types of props, new rules for their placement and new types of textures.

The visibility detection algorithm would also need to be improved to work with more complexe shapes. 