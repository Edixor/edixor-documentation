# Functions

### 1. Define a Custom Function Subclass

```csharp
using ExTools;

public class MyFunction : ExFunction
{
    public override void Activate()
    {
        // Custom logic
    }
}
```

Save this as `MyFunction.cs`.

***

### 2. Create and Configure the `FunctionData` Asset

1. In the Unity **Project** window, right‑click → **Create → Edixor → Function**.
2. Name the asset **MyFunction**.
3. In its Inspector, set:
   * **Name**: MyFunction
   * **Description**: _(optional)_
   * **Icon**: _(optional)_
   * **Enable**: ✓
   * **Script Logic**: select the `MyFunction` MonoScript

Unity will validate that `MyFunction` inherits from `ExFunction`.

***

### 3. Register Your Function in `Configure`

```csharp
protected override void Configure(WindowConfigurationBuilder builder)
{
    builder
        .Function("MyFunction");
}
```

At runtime this will add a **MyFunction** button to the bottom panel, invoking your `MyFunction.Activate()` logic.
