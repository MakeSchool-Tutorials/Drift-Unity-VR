---
title: "Adding Levels Better-er"
slug: a-whole-new-level-better-er
---

You’ve already guessed the answer ;)

Unity gives us an incredible amount of access to write code to do all sorts of things in the Editor, and, as you may have guessed where this is headed, one of these is editing the Build Settings!

You can do this by setting the array of `EditorBuildSettingsScenes` called `EditorBuildSettings.scenes`.

You can create a new `EditorBuildSettingsScene` like this:

```
EditorBuildSettingsScene scene = new EditorBuildSettingsScene(path,true);
```

where path is the file path to the scene name, and you can get the name of a file by calling `file.Name` and then append `"Assets/Levels/"` to the beginning using `+`.

The code gave us a bit of trouble to get right the first time, so we recommend peeking at the solution, but if you want to challenge yourself to write it without, feel free!

> [solution]
>
```
using UnityEditor;
using UnityEngine;
using System.Collections;
using System.Xml;
using System.IO;
using System.Linq;
using System.Collections.Generic;
>
[InitializeOnLoad]
public class OnBuild : object
{
>
    static OnBuild()
    {
>
        EditorApplication.playmodeStateChanged = () => {
>
            if (EditorApplication.isPlayingOrWillChangePlaymode)
            {
                var levelsFolder = "Assets/Levels/";
                DirectoryInfo info = new DirectoryInfo(levelsFolder);
                FileInfo[] files = info.GetFiles();
                files.OrderBy(f => f.Name);
>
                HashSet<string> paths = new HashSet<string>();
>
                foreach (EditorBuildSettingsScene scene in EditorBuildSettings.scenes)
                {
                    string path = scene.path;
                    paths.Add(path);
                }
>
                XmlDocument xmlDoc = new XmlDocument();
                XmlNode rootNode = xmlDoc.CreateElement("levels");
                xmlDoc.AppendChild(rootNode);
>
                string extension = ".unity";
>
                foreach (FileInfo file in files)
                {
                    string currentExtension = file.Extension;
                    if (currentExtension.Equals(extension))
                    {
                        string path = levelsFolder + file.Name;
                        if (!paths.Contains(path))
                        {
                            paths.Add(path);
                        }
                        XmlNode levelNode = xmlDoc.CreateElement("level");
                        levelNode.InnerText = file.Name.Replace(extension, "");
                        rootNode.AppendChild(levelNode);
                    }
                }
>
                xmlDoc.Save("Assets/Resources/levels.xml");
>
                EditorBuildSettingsScene[] scenesNew = new EditorBuildSettingsScene[paths.Count];
>
                int i = 0;
                foreach (string path in paths)
                {
                    EditorBuildSettingsScene scene = new EditorBuildSettingsScene(path, true);
                    scenesNew[i] = scene;
                    ++i;
                }
>
                if (scenesNew != null)
                {
                    EditorBuildSettings.scenes = scenesNew;
                }
            }
        };
>
    }
}
```

Now your level designer can create levels without even needing to open the Build Settings menu!

If we were satisfied now, we could tell our Main Scene to load `Level1` instead of `Play`, and we’d be golden.

However, we want to do even better.
