# Indoor Scene Generator Toolkit

ISGT is a Unity project aimed at generating realistic virtual rooms. They are then used to create datasets filled with screenshots and the information about each window or door in the image. These datasets can be used to train image recognition AIs. 

## How to use 

To use the software, you only need to download the [latest realease](https://github.com/numediart/ISGT/releases), unzip it and start ISGT.exe.

You can change the settings and the output folder via the menu, and then start the process. It can be stopped at any point by pressing escape.

By default, screenshots and JSONs are saved at /ISGT_Data/Export/.

## Parameters

The process is fully parametrable thanks to the following settings : 
- **General settings**
    - Number of room 
    - Screenshots per room
    - Image resolution : the size of the screenshots, from 640 x 360 to 2560 x 1440 
<br>
- **Generation settings**
    - Rooms max size
    - Props density : the amount of furniture in the room
    - Doors density : the max amount of doors per room
    - Windows density : the max amount of windows in the room
<br>
- **Camera settings**
    - Camera max rotation : the max angle of the camera around x,y and z
    - FOV : the diagonal Field Of View of the camera
    - ISO 
    - Aperture
    - Focus Distance
<br>
- **Precision settings**
    - Raycasts amount : the number of rays shot when calculating camera visibility

#### Performances

Most of these parameters (except from camera settings) have an impact on performances and will affect the duration of the process. The estimated remaining time is displayed on the bottom left corner of the screen. 

#### Presets

When applying new settings, a preset file is created and can then be selected to retrieve the same configuration. They can be found in the Ressource folder of the app if the user need to share or delete one. 

## Generated data

Each screenshot is matched with a Json containing info about the room, the seeds, the camera settings and about each opening in the image, namely doors and windows. Here is an example : 
```js
{
  "CameraData": {
    "FieldOfView": 52.2338448,
    "NearClipPlane": 0.3,
    "FarClipPlane": 1000.0,
    "ViewportRectX": 0.0,
    "ViewportRectY": 0.0,
    "ViewportRectWidth": 1920,
    "ViewportRectHeight": 1080,
    "Depth": -1.0,
    "IsOrthographic": false
  },
  "ScreenshotData": {
    "OpeningsData": [
      {
        "Type": "Window",
        "Dimensions": {
          "Height": 1.603815,
          "Width": 1.13412368,
          "Thickness": 0.10204263
        },
        "DistanceToCamera": 7.12805271,
        "RotationQuaternionFromCamera": {
          "w": 0.457306623,
          "x": -0.004237673,
          "y": 0.8892608,
          "z": 0.008240416
        },
        "OpenessDegree": 0.6515185,
        "VisibilityRatio": 0.9289916,
        "BoundingBox": {
          "Origin": [
            1118,
            454
          ],
          "Dimension": [
            118,
            205
          ]
        },
        "VisibilityBoundingBox": {
          "Origin": [
            1120,
            458
          ],
          "Dimension": [
            116,
            200
          ]
        }
      },
    ],
    "CameraRotation": {
      "w": 0.5645235,
      "x": 0.0,
      "y": 0.825417,
      "z": 0.0
    }
  }
}
```

## Work on ISGT

You can fork the [GitHub repository](https://github.com/numediart/ISGT) and start helping us improve the tool.

You can find the technical documentation [here](./Doc/doc-menu.md).