# EdixorWindow

### Creating a Custom Edixor Window

To create a custom **EdixorWindow**, you need to define a class that inherits from it:

```csharp
using ExTools;

public class MyPluginWindow : EdixorWindow 
{
    [MenuItem("Edixor/Window/MyPluginWindow")]
    public static void ShowExample() => ShowWindow<MyPluginWindow>("Cool Window");
}
```

You now have your own Edixor window, which can be opened from the Unity menu:

📁 `Edixor/Window/MyPluginWindow`

This window includes the default tabs and layout.\
To customize it — for example, to define hotkeys or add custom logic — override the `Configure` method:

```csharp
using ExTools;

public class MyPluginWindow : EdixorWindow 
{
    [MenuItem("Edixor/Window/MyPluginWindow")]
    public static void ShowExample() => ShowWindow<MyPluginWindow>("Cool Window");

    protected override void Configure(WindowConfigurationBuilder builder)
    {
        builder
            .Function("Restart")
            .Status("Key Combination")
            // ...
            .BasicTab("NewTab");
    }
}
```

Using the `builder`, you can define various window configurations.\
Here are the available configuration types:

#### 🛠️ Functions

Buttons in the bottom panel that trigger custom logic, [_create function_](../configuration/functions.md)_._&#x20;

#### 🎹 HotKeys

Keyboard shortcuts that trigger actions, [_create HotKeys_](../configuration/hotkeys.md).&#x20;

#### 📊 Status

Status bar items for showing runtime information, [_create status_](../configuration/statuses.md)_._

🎨🧩 **Style & Layout**\
Define the visual appearance and structural layout of the window.\
This includes choosing a UI theme and arranging how panels and tabs are organized by default. [_create_](../configuration/styles-layouts.md)

#### ⚙️ Miscellaneous

Other configuration options like the default window size.\
More [_details._](../configuration/other.md)

You can mix and match these configuration elements to fully tailor your window.

***

#### 🧬 Default Configuration

If you don’t override the `Configure` method, your window will include the following default setup:

```csharp
protected virtual void Configure(WindowConfigurationBuilder builder)
{
    builder
        .HotKey("Exit")
        .HotKey("Minimize")
        .HotKey("Restart")

        .Function("HotKey")
        .Function("Setting")
        .Function("Restart")
        .Function("Minimization")

        .Status("Version")
        .Status("Key Combination")

        .BasicTab("NewTab")

        .Layout("Standard")
        .Style("Unity");
}
```

### Advanced Behavior: Controllers, Settings, and State

In addition to basic configuration, you can also define custom behavior for your Edixor window using various built-in systems.

***

#### 🧠 Controllers

`Controllers` allow you to dynamically control the window during its lifecycle — for example, adding tabs, hotkeys, or status bars at runtime.

Here’s an example of how to add a tab when the window is enabled:

```csharp
public class MyPluginWindow : EdixorWindow
{
    [MenuItem("Edixor/Window/MyPluginWindow")]
    public static void ShowExample() => ShowWindow<MyPluginWindow>("Cool Window");

    // Example: Add a tab via EdixorTab logic
    public void AddDemoTab()
    {
        var demoTab = new DemonstrationTab();
        Controllers.AddTab(demoTab, saveState: true, autoSwitch: true);
    }

    private void OnEnable()
    {
        AddDemoTab();
    }
}
```

***

#### ⚙️ Settings

The `Settings` property provides access to specialized `ScriptableObject` assets associated with your window.

For example, you can access and print all registered statuses:

```csharp
public void PrintAllStatuses()
{
    var settings = Settings;
    if (settings != null && settings.StatusSetting != null)
    {
        StatusData[] statusDatas = settings.StatusSetting.GetAllItem();

        foreach (var statusData in statusDatas)
        {
            Debug.Log($"Status: {statusData.Name}, Description: {statusData.Description}, Enabled: {statusData.Enable}");
        }
    }
    else
    {
        Debug.LogWarning("StatusSetting is not available!");
    }
}

private void OnEnable()
{
    PrintAllStatuses();
}
```

***

#### 🪟 WindowStateSetting

The `WindowStateSetting` class works specifically with `EdixorWindow`.\
It stores window-related data like position, size, state, and more.

You can use it, for instance, to minimize the window programmatically:

```csharp
WindowStateSetting.SetMinimized(true);
```

***

By combining `Controllers`, `Settings`, and `WindowStateSetting`, you gain full control over your window's lifecycle, layout, behavior, and editor time logic.
