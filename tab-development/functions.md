# Working with configurations

### Working with Configurations from Inside a Tab

Every `EdixorTab` has access to the shared `Controllers` facade, so you can dynamically add functions, hotkeys, statuses or even spawn other tabs during its lifecycle.

***

#### 1. Loading HotKeys, Functions & Statuses

Use the `Controllers` methods **before** the facade is initialized (i.e. during `Awake` or `Start`) to register assets from your `Settings`:

```csharp
public class MyTab : EdixorTab
{
    // Called once when the tab is first instantiated
    private void Awake()
    {
        // Load a hotkey asset at path "HotKeys/MyHotKey.asset"
        // and bind it to this tab’s Activate() method
        Controllers.LoadHotKey("MyHotKey", title: Title, action: InvokeUpdateGUI);

        // Load a function asset and bind to a toolbar button
        Controllers.LoadFunction("MyFunction", key: Title, action: () => Debug.Log("Function pressed"));

        // Load a status asset and bind to a status bar
        Controllers.LoadStatus("MyStatus", key: Title, action: null);
    }

    // Standard Enable callback—Controllers.InitOptions() must be called first
    private void OnEnable()
    {
        // Optionally, add a completely new tab at runtime
        Controllers.AddTab(new DemonstrationTab(), saveState: true, autoSwitch: false);
    }
}
```

### 2. API Reference

Method What it Does Controllers.LoadHotKey(name, title, action) Registers a KeyActionData named name under title title, binding action. Controllers.LoadFunction(name, key, action) Registers a FunctionData named name with identifier key, binding action. Controllers.LoadStatus(name, key, action) Registers a StatusData named name with identifier key, binding action. Controllers.AddFunction(Function logic) Adds a live Function instance to the toolbar. Controllers.AddStatus(Status logic) Adds a live Status instance to the status bar. Controllers.AddTab(EdixorTab tab, saveState, autoSwitch) Spawns a new tab logic at runtime.

Note:

All LoadXxx calls must occur before Controllers.Initialize() completes.

AddXxx methods can be called anytime after initialization.

By using these APIs inside your tab’s lifecycle callbacks (Awake, Start, OnEnable, etc.), you can fully customize which hotkeys, toolbar buttons, status items or even additional tabs appear and when.
