---
title: "Coursing Through The Wind"
slug: whooooooooooooosh
---

Our game would look a lot better with some sort of effect to make it
look like we were coursing forward, making wakes in the wind.

To do this, we’re going to use a Particle System.

>[action]
>Add a Particle System Game Object as a child of Main Camera (head).

![We added a particle system](../media/image85.png)

>[action]
>Set its y rotation to 180 and z position to 5, so that it faces the camera. Then set it to prewarm, set its Start Lifetime to 1, Emission Rate to 100, and Radius to 0.01. To access the Emission and Shape properties, click the boxes to expand them.

![We made the system face the camera and be offset in front of it](../media/image127.png)

>[action]
>To make the effect look better, check and expand the Size Over Lifetime and Color Over Lifetime panels.

![Panels in the Particle System Inspector](../media/image82.png)

>[action]
>Click the white box next to Color to open up the Gradient Editor.

The little pointers on the top represent alpha, and the ones on the bottom represent hue. You can click to create new ones, and move or delete existing ones. Click on the top right one and set its alpha to 0.

![We made the particles fade in color from birth until death](../media/image121.png)

This will make each particle start tinted white and end clear.

>[action]
>Now click on the grey box next to Size. This is the curve that represents the size. You can add or edit key points on the curve. Grab the farthest left one and drag it down to make a pretty curve.

![Size over lifetime can be represented as a curve](../media/image36.png)

This will make each particle start tiny and end big.

Together with the color shift effect, we get particles that appear to
expand into nothingness.

![The particles come toward us and then fade away](../media/image126.gif)

As a final touch to our particles, we’ll make the particles thinner.

>[action]
>In the Render section, change the Render Mode to Stretched Billboard, then set the Length Scale to -10 and the Speed Scale to -0.5.

![Changing the render mode makes the particles thinner](../media/image31.png)

Now the Particle System should look pretty great! To prevent some
clipping, we also moved the system back to z position 10.

![We changed certain parameters to make our effect look better](../media/image135.gif)

Now when you fly around, you should see a wake around you. If you want it to look a little less tethered to your face, change the Simulation Space from Local to World. See how this change affects the way it feels. You might like one better than the other.

![We thought the particles looked best in world space](../media/image118.png)
