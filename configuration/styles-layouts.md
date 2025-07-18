# Styles & Layouts

## Styling Edixor Windows & Inspectors

Edixor’s styling system is driven by **StyleData** assets and parameter sets. There are two built‑in parameter types:

* **EdixorParameters** — contains all the box, label, button, tab and function styles used by `EdixorWindow` and `EdixorInspector`.
* **MenuParameters** — defines colors for menus (background, hover, selected).

### How It Works

1. A **StyleData** asset holds one or more `StyleParameters` (e.g. `EdixorParameters`, `MenuParameters`).
2. At runtime, Edixor looks for your `StyleData` by name and applies its parameters to all UI elements.
3.  In your window or inspector’s `Configure` method, you choose which style to use via:

    ```csharp
    builder
      .Style("MyStyleName");
    ```

### 1. Create a `StyleData` Asset

1. In Unity’s **Project** window, right‑click → **Create → Edixor → Style → Data**.
2. Name it **MyStyle** (this is the name you’ll use in `builder.Style("MyStyle")`).
3. In its Inspector you’ll see an **Asset Parameters** array. Click **+** and choose the parameter type:
   * **EdixorParameters** (for window/inspector styling)
   * **MenuParameters** (for menus only)
4. Configure your parameters:
   * For **EdixorParameters**: adjust arrays of `BoxStyleEntry`, `LabelStyleEntry`, `ButtonStyleEntry`, `ExTabStyle`, `ExFunctionStyle`, etc.
   * For **MenuParameters**: set `MenuBackgroundColor`, `ItemHoverColor`, `ItemSelectedColor`.
5. Save the asset under a folder named **ExStyle** (so Edixor will auto‑discover it).

### 2. Add Your Style Folder

Place your `MyStyle.asset` inside any `ExStyle` folder in your project, for example:

```
MyPlugin/ExStyle/MyStyle.asset
```

Edixor scans all `ExStyle` folders at startup.

### 3. Apply Your Style in `Configure`

```csharp
protected override void Configure(WindowConfigurationBuilder builder)
{
    builder
        // other configs …
        .Style("MyStyle");     // matches your StyleData.Name
}
```

At runtime, Edixor will:

* Load your `StyleData` → retrieve its `EdixorParameters` and/or `MenuParameters`
* Call `ApplyTo(...)` on every UI element according to the chosen state (normal, hover, active, etc.)

***

With these steps, you can create and register any number of custom visual themes for both **EdixorWindow** and **EdixorInspector**.
