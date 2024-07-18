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

To test the accuracy of the bounding boxes (full ones and visibility ones), we generated a data set of 800 screenshots taken by taking 10 screenshots in 80 randomly generated rooms. We then traced the bounding boxes on the images using the OpenCV library and this python script :

```python
import cv2
import json
import os

def draw_bounding_boxes(image_path, json_data, output_dir):
    # Load image
    image = cv2.imread(image_path)
    height, width, _ = image.shape

    # Access OpeningsData inside ScreenshotData
    openings_data = json_data["ScreenshotData"]["OpeningsData"]
    
    # Loop through each window in the OpeningsData
    for window in openings_data:
        # Draw bounding box
        draw_rectangle(image, window["BoundingBox"], (0, 255, 0), height)
        # Draw visibility bounding box
        draw_rectangle(image, window["VisibilityBoundingBox"], (0, 0, 255), height)

    # Construct the path for the annotated image
    base_name = os.path.basename(image_path)
    annotated_image_path = os.path.join(output_dir, base_name.replace('.png', '_annotated.png'))

    # Save the annotated image
    cv2.imwrite(annotated_image_path, image)

def draw_rectangle(image, bounding_box, color, image_height):
    origin = bounding_box["Origin"]
    dimension = bounding_box["Dimension"]
    x, y = origin
    w, h = dimension

    # Invert y-coordinate
    y = image_height - y - h

    cv2.rectangle(image, (x, y), (x + w, y + h), color, 2)

def process_directory(images_dir, jsons_dir, output_dir):
    # Create the output directory if it does not exist
    if not os.path.exists(output_dir):
        os.makedirs(output_dir)

    for root, dirs, files in os.walk(jsons_dir):
        for json_file in files:
            if json_file.endswith('.json'):
                json_path = os.path.join(root, json_file)
                relative_path = os.path.relpath(root, jsons_dir)
                image_subdir = os.path.join(images_dir, relative_path)
                image_file = json_file.replace('.json', '.png')
                image_path = os.path.join(image_subdir, image_file)
                if os.path.exists(image_path):
                    with open(json_path, 'r') as f:
                        json_data = json.load(f)
                    sub_output_dir = os.path.join(output_dir, relative_path)
                    if not os.path.exists(sub_output_dir):
                        os.makedirs(sub_output_dir)
                    draw_bounding_boxes(image_path, json_data, sub_output_dir)
                else:
                    print(f"Image file for {json_file} not found in {image_subdir}.")

# Directory paths
images_dir = # Path to images directory
jsons_dir = # Path to JSON files directory
output_dir =  # Path to output directory

# Process all images and JSON files
process_directory(images_dir, jsons_dir, output_dir)
```

We then manually checked the accuracy of the bounding boxes by comparing the annotated images with the original ones. We considered the test to be successful if the bounding boxes were correctly placed around the openings.

On the 800 images, we detected 22 incorect bounding boxes, which is a 97.25% success rate. The failures were caused by incorrect colliders on some props, resulting in hiding openings from the camera but not from our eyes. All colliders were then fixed and the test was rerun with a 100% success rate.


## Benchmarking

