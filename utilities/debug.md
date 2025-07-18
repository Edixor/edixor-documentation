# Debug

## EdixorObjectLocator

**EdixorObjectLocator** is a utility class used to work with virtual paths and asset resolution in Edixor. It provides an aliasing system for asset folders and simplifies asset loading across the project.

***

### 🔧 Key Features

*   **Load assets using aliases**:

    ```csharp
    LoadObject<T>(string virtualPath)
    ```

    Loads an object of type `T` from a virtual path like `"SettingAsset/MySettings"`.
*   **Search assets in folders**:

    ```csharp
    FindAssetsInFolder<T>(string virtualPath, Func<T, bool> filter = null)
    ```

    Finds and filters assets within the folder specified by the virtual path.
*   **Create instances from assets**:

    ```csharp
    FindAndCreateInstances<TFind, TCreate>(string virtualPath)
    ```

    Finds assets of type `TFind` and instantiates `TCreate` based on them.
*   **Import external directories**:

    ```csharp
    ImportDirectory(string sourcePath, string virtualTargetPath)
    ```

    Copies a directory from outside the project into the target virtual folder.
*   **Retrieve root Edixor folder**:

    ```csharp
    GetEdixorRootFolder()
    ```

    Resolves the base folder of the Edixor system (cached internally).

***

### 📁 Alias Mapping

The locator supports a set of aliases that map to asset directories in the project:

| Alias            | Path                                |
| ---------------- | ----------------------------------- |
| Servers          | Editor/Core/Services                |
| SettingAsset     | Editor/Settings/Assets              |
| Extensions       | Editor/Extensions/Data              |
| Functions        | Editor/Core/Functions/Data          |
| Status           | Editor/Core/Status/Data             |
| Tabs             | Editor/UI/Tabs/Data/Basic           |
| WindowStates     | Editor/UI/UIEdixor/Windows/States   |
| InspectionStates | Editor/UI/UIEdixor/Inspector/States |

***

### ⚠️ Notes

* Internally uses `AssetDatabase` and is only available in the Unity Editor.
* Commonly used by controllers and configuration systems across Edixor.
* Folder aliases are case-sensitive and must match the internal dictionary.

***

### 🧩 Used In

* Configuration loading (e.g., `SettingAsset`, `Status`)
* Tab system (`Tabs`, `WindowStates`, `InspectionStates`)
* Dynamic UI and inspector construction
* External directory imports (e.g., examples or templates)

## EdixorDebug — Internal Debug System for Edixor

`EdixorDebug` is an internal logging system designed to **replace `Debug.Log` inside Edixor** to **avoid cluttering the Unity Console**. All messages are stored separately and can be viewed through the Edixor Debug tab.

***

### Purpose

* Stores logs separately from the Unity Console
* Supports **grouping** of messages
* Allows toggling Unity Console output (disabled by default)
* Automatically captures **stack trace**, **calling method**, and **line number**

***

### Usage

#### Initialization

```csharp
ExDebug.InitSetting(console: true); // Enable output to Unity Console
```

Logging

```csharp
ExDebug.Log("Info message");
ExDebug.LogWarning("Warning message");
ExDebug.LogError("Error message");
ExDebug.LogComment("Comment message");
```

Grouping

```csharp
ExDebug.BeginGroup("Data Import");
ExDebug.Log("Loading completed");
ExDebug.EndGroup();
```

History Management

```csharp
var allMessages = ExDebug.GetHistory();  // Get all logged messages
ExDebug.ClearHistory();                   // Clear the log history
```

Notes Messages can be nested inside groups and will be visualized later inside the Edixor Debug tab UI.
