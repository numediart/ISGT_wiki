# Testing

This section is dedicated to the testing of the project. It will cover the different features that have been teste by describing the protocols used and the results obtained.

## Distance calculation

To test the distance calculation, we generated different roooms of various sizes and kept the seeds for reproducibility. We then chose points in the rooms to place the camera and measured the distance between the point and the openings using this [measuring tool](https://assetstore.unity.com/packages/tools/utilities/measuring-tool-226340) available on the Unity Asset Store.

We then ran the process by forcing the camera to be placed at the chosen points and compared the distances obtained with the ones measured. If the difference was less than 1 cm, we considered the test to be successful.

We used pair wise testing to cover as much case as possible with the least amount of tests. The two variables were : 
- Distance : ~25m, ~15m, ~5m, ~2m
- Position of the opening in the image 

| Distance | Position | Result |
|----------|----------|--------|
| 25m      | Center   | Success|
| 25m      | Right    | Success|
| 25m      | Bottom   | Success|
| 15m      | Center   | Success|
| 15m      | Left     | Failure|
| 15m      | Top      | Success|
| 5m       | Center   | Success|
| 5m       | Top right| Success|
| 5m       | Bottom   | Success|
| 2m       | Center   | Success|
| 2m       | Bottom left| Success|
| 2m       | Top      | Success|

The only failure was caused by the fact that only a very small portion of the opening was visible in the image, so it was not detected because the precision was to low and we ran the test, thus no distance was measured. So we can conclude that the distance calculation is accurate.


## Angle calculation

To test the angle calculation, we used the same rooms and points as for the distance calculation. We measured each angle between the camera and the openings with our script, and we applied the rotation to the camera. If the center of the opening game object was contained in a 20 pixels wide square in the center of the game view, we considered the test to be successful.

Here are the results :

| Distance | Position | Result |
|----------|----------|--------|
| 25m      | Center   | Success|
| 25m      | Right    | Success|
| 25m      | Bottom   | Success|
| 15m      | Center   | Success|
| 15m      | Left     | Failure|
| 15m      | Top      | Success|
| 5m       | Center   | Success|
| 5m       | Top right| Success|
| 5m       | Bottom   | Success|
| 2m       | Center   | Success|
| 2m       | Bottom left| Success|
| 2m       | Top      | Success|

The only failure was caused by the same reason, the opening was not detected because so no angle was measured. We can conclude that the angle calculation is accurate.

## Bounding boxes

## Benchmarking

