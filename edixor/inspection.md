# EdixorInspection

## EdixorInspector

### Creating a Custom Edixor Inspector

To create a custom **EdixorInspector**, define a class inheriting from it and decorate with `CustomEditor`:

```csharp
using UnityEditor;
using ExTools;

[CustomEditor(typeof(MyInspectTarget))]
public class MyInspectWindow : EdixorInspector
{
    public override VisualElement CreateInspectorGUI()
    {
        return base.CreateInspectorGUI();
    }
}
```

Your inspector becomes available when selecting an object of type `MyInspectTarget` in Unity.

### Basic Configuration

Override the `Configure` method to specify hotkeys, functions, statuses, tabs, layout, and style:

```csharp
protected override void Configure(WindowConfigurationBuilder builder)
{
    builder
        .HotKey("Exit")
        .HotKey("Restart")
        .Function("HotKey")
        .Function("Setting")
        .Function("Restart")
        .Status("Version")
        .Status("Key Combination")
        .BasicTab("ExtensionInspectTab")
        .Layout("Standard")
        .Style("Unity");
}
```

#### Configuration Types

* **Functions**: Buttons in the bottom panel
* **HotKeys**: Keyboard shortcuts
* **Statuses**: Status bar items
* **Tabs**: Predefined content areas (e.g., `ExtensionInspectTab`)
* **Layout**: Window arrangement scheme
* **Style**: UI theme
* **Miscellaneous**: Other options (e.g., default size)

### Default Configuration

If not overridden, the default setup is applied:

```csharp
protected virtual void Configure(WindowConfigurationBuilder builder)
{
    builder
        .HotKey("Exit")
        .HotKey("Restart")
        .Function("HotKey")
        .Function("Setting")
        .Function("Restart")
        .Status("Version")
        .Status("Key Combination")
        .BasicTab("ExtensionInspectTab")
        .Layout("Standard")
        .Style("Unity");
}
```

### Advanced Behavior

#### 🧠 Controllers

Use `Controllers` to add tabs or actions during lifecycle events. For example:

```csharp
public class MyInspectWindow : EdixorInspector
{
    private void OnEnable()
    {
        var tab = new DemonstrationTab();
        Controllers.AddTab(tab, true, true);
    }
}
```

#### ⚙️ Settings

Access `Settings` to work with ScriptableObject assets. For example, printing all statuses:

```csharp
public void PrintStatuses()
{
    var settings = Settings;
    if (settings != null && settings.StatusSetting != null)
    {
        var items = settings.StatusSetting.GetAllItem();
        foreach (var status in items)
        {
            Debug.Log($"{status.Name} | {status.Description} | {status.Enable}");
        }
    }
}
```

#### 🪟 WindowStateSetting

Control window state (position, size, minimized) using `WindowStateSetting`:

```csharp
WindowStateSetting.SetMinimized(true);
```

### Custom Main Tab

Override `OnMainTab` to define the main UI content for the inspector:

```csharp
protected override void OnMainTab(VisualElement mainTab)
{
    mainTab.Add(new Label("Custom inspector content"));
    mainTab.Add(new Button(() => { }));
}
```

This content appears under the `ExtensionInspectTab` when configured in `Configure`.

***

By following this template, you can create and customize your own **EdixorInspector** with rich editor UI and behavior.
