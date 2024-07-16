# Database Generation
## Save Camera view

In the runtime, the main camera view will be automatically saved in the path you specify. The camera view will be saved as a .png file with the date and time as the file name.
There is a parameter where you can set the number of screenshots you want to save per room. The default value is 5.

## Camera parameters

The camera can be adjusted with the following parameters:

- **Field of view**: The field of view of the camera. The default value is 90.
- **ISO**: The ISO of the camera.
- **Focus distance**: The shutter speed of the camera.
- **Aperture**: The aperture of the camera. The default value is 5.6.

- **Camera rotation**: Define the maximum rotation of the camera in degrees. By default, the camera will on all axes.

## Take a screenshot

To take a screenshot of the camera view, you can use the following code:

```Csharp
using UnityEngine;
using CameraScreenshot;

public class CameraScreenshotExample : MonoBehaviour
{
    [SerializeField] private CameraScreenshot cameraScreenshot;
    void Start()
    {
        cameraScreenshot.savePath = Application.dataPath + "/yourPath";
        cameraScreenshot.CaptureScreenshot();
    }
}
```

