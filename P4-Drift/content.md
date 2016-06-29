---
title: "Controlling Time"
slug: time-lord
---

As a final gameplay step, we want to add a slow-down time mechanic.

To do this, we’ll need a reference to a SteamVR\_TrackedController in our Scene.

>[action]
>Create a new component named TimeController, and make it require a SteamVR\_TrackedController component and set that component’s index to 1 when it’s added.

<!-- -->

>[solution]
>
>Our code to do this looks like this:
>
```
using UnityEngine;
using System.Collections;
>
[RequireComponent(typeof(SteamVR_TrackedController))]
public class TimeController : MonoBehaviour {
>
  // Use this for initialization
  void Start () {
>
  }
>
  void Reset() {
    SteamVR_TrackedController controller = GetComponent<SteamVR_TrackedController>();
    controller.SetDeviceIndex(1);
  }
>
  // Update is called once per frame
  void Update () {
>
  }
>
}
```

<!-- -->

>[action]
>Now create an Empty Game Object named TimeController and add the TimeController component to it. This should add and set a SteamVR\_TrackedController component, as expected.

![](../media/image131.png)

Now we can write code in our TimeController component to read the inputs from the SteamVR\_TrackedController to make time slow down when we squeeze the controller!

To double-check that this component is, in fact, reading inputs from the Vive controller, by the way, run the Scene and try pressing some of the buttons on the controller. You should see the appropriate check boxes fill in on the component when you do.

A simple way we could freeze time would be to do the following in our update method:

```
SteamVR_TrackedController controller = GetComponent<SteamVR_TrackedController>();
Time.timeScale = controller.gripped ? 0 : 1;
```

This will bring everything to a halt when you squeeze your controller, but it's rather abrupt. How can we slow it down gradually?

One strategy would be to slowly add or subtract some small number to Time.timeScale each frame in the following way:

```
float dTimeScale = timeScaleRate * amountOfTimeThatPassedThisFrame;
if (controller.gripped) {dTimeScale *= -1;}
Time.timeScale = Mathf.Clamp01(Time.timeScale + dTimeScale);
```

where timeScaleRate is some public variable and amountOfTimeThatPassedThisFrame is how much time passed this frame, and where Mathf.Clamp01 bounds the return value between 0 and 1.

Unfortunately, we cannot use Time.deltaTime as amountOfTimeThatPassedThisFrame, because when we change Time.timeScale, that also scales Time.deltaTime, and so when Time.timeScale reachers 0, Time.deltaTime will always be 0.

Fortunately, Unity provides us a metric for how much time passed each frame that does NOT scale with Time.timeScale. That’s Time.unscaledDeltaTime.

>[action]
>With that knowledge, make time slow down gradually when the player grips the controller, and then gradually come back to normal speed when the player un-grips.

![](../media/image123.gif)

>[solution]
>
>We’ve modified our TimeController component to look like this:
>
```
using UnityEngine;
using System.Collections;
>
[RequireComponent(typeof(SteamVR_TrackedController))]
public class TimeController : MonoBehaviour {
>
  public float timeScaleRate;
  private SteamVR_TrackedController controller;
>
  private void Initialize() {
    controller = GetComponent<SteamVR_TrackedController>();
  }
>
  // Use this for initialization
  void Start () {
    Initialize();
  }
>
  void Reset() {
    Initialize();
    controller.SetDeviceIndex(1);
  }
>
  // Update is called once per frame
  void Update () {
>
    float dTimeScale = timeScaleRate * Time.unscaledDeltaTime;
    if (controller.gripped) { dTimeScale *= -1; }
    Time.timeScale = Mathf.Clamp01(Time.timeScale + dTimeScale);
  }
}
```