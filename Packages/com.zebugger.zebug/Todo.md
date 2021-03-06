﻿#Todo

## Big Ideas

1) GUI tab for customizing build options and defaults. Auto-add the correct preprocessor
   directives. Dev (on), Perf (off), Prod (stripped). Have logging toggleable from a pref/shortcut
   too, so you don't just have to compile them out. Make sure that you can set a channel and force
   it to be on in a build. A reasonably common use case for a library like this would be to
   replace ```if (m_Debug) { Debug.Log(...); }```, but also 
   ```C#
   #ifdef SUPER_VERBOSE_LOGGING
   Debug.Log(...); 
   #endif
   ```

2)  Togglable Profiling!
    ```C#
    Zebug.Stopwatch.Start()
    ```

3)  Gizmo shapes
    Square, cross, circle, donut? outline shapes? Arrow etc. Possible integration with the SVG
    shapes library would be sweet, with GLLines fallback. Zebug.Draw3Cross(optionally flat on one axis)

4)  ZebugGraph:
    * consider using closures to store the cached channel, that way you can avoid editor only fields and preprocessor directives cluttering up the filespace
    * "Triggering" for graphs? Stop when it gets to a value (etc?)
    * Collapse channels between trigger values? (compare all graphs at time dt, based on channel X exceeding value)

    ```C#
    Zebug.GraphValue("Graph Name", currentDt);
    ```

5)  Component to drop onto a transform and design-time customize a Gizmo for a selected channel
    (Allow creation of channels via ScriptableObject) (have something run on-load, finds type of Channel
    from shared project type cache (spin up as separate package)

6)  Utility extension for GameObject.FullName()

7)  Optionally Log to Window, keep track of things in an inspector, rather than flooded into console

8)  Log/Gizmo enabled should be flags, esp if GUIButton gets added

9)  Zebug.GUIButton
    
    On startup (Awake etc?), add your buttons, and callbacks to hook into, then a class can add debug hooks
    for that kind of behaviour, and the main Zebug class can handle all the annoying layout
    and enable/disable stuff.

    Static, or per last-selected object

    ```C#
    private void OnGUI() {
        if (channel.DebugGUIEnabled) {
            for (int i = Zebug.s_Buttons.Count - 1; i >= 0; i--) {
                Button b = Zebug.s_Buttons[i];
    
                Rect buttonRect = Zebug.LayoutNextGUIButton(b);
                if (GUI.Button(buttonRect, b.name) {
                    b.callback?.Invoke();
                }
            }
        }
    }
    ```
 10)  Option for displaying logs in-game?

## Misc Tasks
* Cache assembly types on-reload of assemblies, extract this to a separate package, so projects can
  share the type information without duplicating the work. (Finding network RPCs for example)
    
* `Channel` should be probably be renamed `ZebugChannel`, as it's used naked and
  has no context when you read it in an inheritance declaration.
    
* Move ColorTagsOnlyInEditor to some sort of true library config