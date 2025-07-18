# Hotkeys

### 1. Define a Custom HotKey Subclass

```csharp
using ExTools;

public class MyHotKey : ExHotKey
{
    public override void Activate()
    {
        // Custom logic
    }
}
```

Save this as `MyHotKey.cs`.

***

### 2. Create and Configure the `KeyActionData` Asset

1. In the Unity **Project** window, right‑click → **Create → Edixor → HK**.
2. Name the asset **MyHotKey**.
3. In its Inspector, set:
   * **Name**: MyHotKey
   * **Enable**: ✓
   * **Combination**: select one or more `KeyCode` entries (e.g. `KeyCode.R`)
   * **Script Logic**: assign the `MyHotKey` MonoScript

Unity will validate that `MyHotKey` inherits from `ExHotKey`.

***

### 3. Register Your HotKey in `Configure`

```csharp
protected override void Configure(WindowConfigurationBuilder builder)
{
    builder
        .HotKey("MyHotKey");
}
```

At runtime this will bind your specified key combination to invoke `MyHotKey.Activate()`.
