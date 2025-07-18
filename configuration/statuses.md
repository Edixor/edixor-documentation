# Statuses

## Setting Up Status Items in Edixor

### ExStatus Overview & Lifecycle

The `ExStatus` class lets you add dynamic status items to the status bar. It:

* Loads and injects a UI element (`VisualElement`) via `LoadUI()`
* Manages show/hide on the status bar container
* Supports Unity‑style lifecycle callbacks on your subclass:
  * **Awake** – called once when the status is first initialized
  * **Start** – called once after `Awake` (if implemented)
  * **OnEnable** – called each time the status is shown
  * **OnDisable** – called each time the status is hidden
  * **OnDestroy** – called when the window closes
  * **Update** – called on every editor update tick

***

### 1. Define a Custom Status Subclass

```csharp
using ExTools;
using UnityEngine.UIElements;

public class MyStatus : ExStatus
{
    private Label _label;
    private VisualElement _item;

    public override VisualElement LoadUI()
    {
        if (_item != null)
            return _item;

        _item = new VisualElement();
        _item.AddToClassList("status-bar-item");

        _label = new Label();
        _item.Add(_label);

        _label.text = "Status: OK";
        return _item;
    }

    public void Awake()
    {
        // runs once when initialized
    }

    public void OnEnable()
    {
        // runs each time the status is shown
    }

    public void Update()
    {
        // runs every editor update
    }
}
```

Save this as `MyStatus.cs`.

***

### 2. Create and Configure the `StatusData` Asset

1. In the Unity **Project** window, right-click → **Create → Edixor → Status**
2. Name the asset **MyStatus**
3. In its Inspector, set:
   * **Name**: MyStatus
   * **Description**: _(optional)_
   * **Enable**: ✓
   * **Script Logic**: assign the `MyStatus` MonoScript

Unity will validate that `MyStatus` inherits from `ExStatus`.

***

### 3. Register Your Status in `Configure`

```csharp
protected override void Configure(WindowConfigurationBuilder builder)
{
    builder
        .Status("MyStatus");
}
```

At runtime, this will add your custom status item to the status bar and hook into all supported lifecycle methods automatically.
