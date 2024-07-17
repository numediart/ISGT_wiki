# Database Generation

After the room is generated, the data is collected in the ```DatabaseGenerator.cs``` script. Each steps will be detailed in the following sections.

## Camera placement

First, all the empty nodes are retrieved from the room's quad tree. The camera is the placed randomly in one of them, and a random rotation is applied according to the settings. We then ensure that the camera is not placed too close to the walls, the ceiling or to props to avoid clipping issues. This is done by creating a collider sphere around it and checking for collisions.



## Save Camera view

Secondly, a screenshot is generated using this custom method in ```CameraScreenshot.cs``` which allow to save the camera view while ignoring the UI. 

```Csharp
private IEnumerator Capture()
        {
            // Create a RenderTexture to save the camera view
            RenderTexture rt = new RenderTexture(imageWidth, imageHeight, 24);
            cameraToCapture.targetTexture = rt;

            // Create a 2D texture to save the screenshot
            Texture2D screenShot = new Texture2D(imageWidth, imageHeight, TextureFormat.RGB24, false);
            cameraToCapture.Render();

            // Activate the RenderTexture and read the pixels
            RenderTexture.active = rt;
            screenShot.ReadPixels(new Rect(0, 0, imageWidth, imageHeight), 0, 0);
            screenShot.Apply();

            // Reset the camera and RenderTexture
            cameraToCapture.targetTexture = null;
            RenderTexture.active = null;
            Destroy(rt);

            // Save the screenshot
            byte[] bytes = screenShot.EncodeToPNG();
            File.WriteAllBytes(savePath, bytes);
            yield return null;
        }
```

## Data collection

Finally, the data about each openings in the image is collected :

#### Distance

The distance from the camera is calculated by substrating the camera position from the opening position.

#### Angle

The quaternion angle between the camera and the opening is calculated using the ```Quaternion.LookRotation()``` method. 

#### Dimensions

The dimensions of the opening are calculated by using the ```Bounds.size``` property of the opening's collider.

#### Bounding boxes

##### Full bounding box

The bounding box is the 2D rectangle that contains the opening. It is calculated by using the ```Bounds.min``` and ```Bounds.max``` properties of the opening's collider in the ```Openings.cs``` script.

##### Visibility bounding box

The visibility bounding box is the 2D rectangle that contains only the visible part of the opening. It is calculated by casting numerous rays from the camera to the opening and finding the intersection points. The bounding box is then calculated using the intersection points in the ```Openings.cs``` script.

````Csharp
public BoundingBox2D GetVisibilityBoundingBox()
    {
        gameObject.TryGetComponent<BoxCollider>(out BoxCollider openingBounds);
        _width = RoomsGenerator.GetOpeningWidth(openingBounds.size);
        _height = openingBounds.size.y;
        int minX = Screen.width + 1;
        int maxX = -1;
        int minY = Screen.height + 1;
        int maxY = -1;

        float widthStep = _width / Mathf.Sqrt(NumberOfPoints);
        float heightStep = _height / Mathf.Sqrt(NumberOfPoints);

        for (float x = -_width / 2f + widthStep / 2; x < _width / 2f; x += widthStep)
        {
            for (float y = -_height / 2f + heightStep / 2; y <= _height / 2f; y += heightStep)
            {
                var thisTransform = transform;
                Vector3 positionOffset = thisTransform.right * x + thisTransform.up * y;
                Vector3 aimPoint = GetCenter() + positionOffset;
                if (IsPointVisible(aimPoint) && IsPointOnScreen(aimPoint))
                {
                    Vector3 screenPoint = _mainCamera.WorldToScreenPoint(aimPoint);
                    minX = (int)Mathf.Min(minX, screenPoint.x);
                    maxX = (int)Mathf.Max(maxX, screenPoint.x);
                    minY = (int)Mathf.Min(minY, screenPoint.y);
                    maxY = (int)Mathf.Max(maxY, screenPoint.y);
                }
            }
        }
        // 640 * 360 is the minimum resolution
        int screenShotWidth = 640 * MainMenuController.PresetData.Resolution;
        int screenShotHeight = 360 * MainMenuController.PresetData.Resolution;
        
        // Scale coordinates to screenshot size
        minX = (int)(minX * screenShotWidth / Screen.width);
        maxX = (int)(maxX * screenShotWidth / Screen.width);
        minY = (int)(minY * screenShotHeight / Screen.height);
        maxY = (int)(maxY * screenShotHeight / Screen.height);


        return new BoundingBox2D(new Vector2Int(minX, minY), maxX - minX, maxY - minY);
    }

    private bool IsPointVisible(Vector3 aimPoint)
    {
        GameObject mainCamera = _mainCamera!.gameObject;
        Vector3 aimPointDirection = aimPoint - mainCamera.transform.position;

        if (Physics.Raycast(mainCamera.transform.position, aimPointDirection, out var hit, float.MaxValue))
        {
            if (hit.collider.gameObject == gameObject || hit.collider.gameObject.transform.parent == transform)
                return true;
        }

        return false;
    }
    
    // Check if a point is on the screen, i.e. in the camera's view frustum
    private bool IsPointOnScreen(Vector3 point)
    {
        Vector3 screenPoint = _mainCamera!.WorldToViewportPoint(point);
        return screenPoint.x is > 0 and < 1 && screenPoint.y is > 0 and < 1 && screenPoint.z > 0;
    }
````

#### Visibility ratio

The visibility ratio is the ratio of the visibility bounding box area over the full bounding box area. The data is only kept if the visibility ratio is different from 0, to avoid collecting data from openings that are not visible in the image.

## Save data

The screenshot is then saved, along with a matching JSON file containing the collected data about each visible openings and the camera information. The JSON file is structured as follows:

```json
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

A JSON is also created for each room, containing the seeds, the room's dimensions, the generation's time and the placement data. 
The seeds can be used to reproduce random generation. 