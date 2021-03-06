---
title: "Making The Menu"
slug: making-the-menu
---

Let’s add a Menu Scene that we’ll go back to when we win.

> [action]
>
Create a new Scene in the Scenes folder named `Main` and remove the Skybox.
>
![The Main Scene in the Project Panel](../media/image105.png)
>
Drag a `SteamVR_Camera` component onto Main Camera. Be sure to position Main Camera at `(0,0,0)`.
>
![The camera set up to be a VR camera](../media/image34.png)
>
Now add a Canvas to the Scene. Set its `Render Mode` to `World Space`, `Scale` it to `0.1` in all dimensions, and set its `position` to `(0,0,100)`.
>
![A world space canvas](../media/image99.png)
>
Now add some Text as a child to it and write something like `Squeeze Grip to Begin.` Format it as you see fit.
>
![Instructions on the menu](../media/image120.png)

Now you should see some text floating in front of you.

![The text should float in front of you](../media/image28.png)

To make the effect a little nicer, we made the Camera’s Background color gray and the text white.

# Starting the game

> [challenge]
>
Now implement functionality to switch to the Play scene when you squeeze the controller.
>
![Set up the scene to switch when you squeeze the grip](../media/image72.gif)
>
Don’t forget to add your Scenes to the Build Settings! (Drag it to `File > Build Settings`)

<!--  -->

> [solution]
>
We created a component called MenuController, on a MenuController GameObject, with the following implementation:
>
```
using UnityEngine;
using System.Collections;
using UnityEngine.SceneManagement;
>
[RequireComponent(typeof(SteamVR_TrackedController))]
public class MenuController : MonoBehaviour
{
    private SteamVR_TrackedController controller;
>
    // Use this for initialization
    void Start()
    {
        Initialize();
    }
>
    void Reset()
    {
        Initialize();
    }
>
    // Update is called once per frame
    void Update()
    {
        if (controller.gripped)
        {
            SceneManager.LoadScene("Play");
        }
    }
>
    void Initialize()
    {
        controller = GetComponent<SteamVR_TrackedController>();
        int rightIndex = SteamVR_Controller.GetDeviceIndex(SteamVR_Controller.DeviceRelation.Rightmost);
        controller.SetDeviceIndex(rightIndex);
    }
}
```

<!-- -->

# Back to the menu

> [challenge]
>
Now add logic to switch back to Main when you beat the level.
>
![Switch back when you beat the game!](../media/image132.gif)

<!--  -->

> [solution]
>
We replaced our log statement with this:
>
```
SceneManager.LoadScene("Main");
return;
```

Now you have a feature-complete one-level game!

> [challenge]
>
Go ahead and make it more interesting than a hallway by changing the path and/or adding some obstacles.
>
![We added trees to make our game pretty!](../media/image79.gif)

We downloaded a package off the Asset Store with low poly trees and put them all over with the Default-Diffuse material! We also imported the Standard Assets Effects package and applied some Bloom to the Main Camera (eye), because Bloom looks oh so pretty. We also thought our lights would look prettier a little brighter and played around with our Particle System to make it more subtle (it’s now a cone and has overall lower opacity) ;)

As a final touch, we added the title, Draft, to the Main Menu, as some pulsing Text.

![Our menu is so pretty :3](../media/image97.gif)

Be sure to save your Scenes and save your project!

Feel free to enjoy this version on its own for a little. Go on to the next part to learn how to add a level structure!
