# Getting Started

### Installation

To begin, install one of the available versions of **Edixor**.\
The latest version is available [here](https://github.com/Edixor/edixor-package/latest)

All other versions can be found in the versions repository: [https://github.com/Edixor/edixor-package](https://github.com/Edixor/edixor-package)\


### Setup

To interact with the system, you’ll need to import the `ExTools` namespace:

```csharp
using ExTools;
```

### Configuration

After unpacking the `.unitypackage` into your project, you can configure Edixor by navigating to:

`Edixor/Editor/Settings/Assets/`

There you’ll find several ScriptableObjects.\
The one we need is **`OtherEdixorSetting`**.

In addition to information about the current build, you’ll find the following configuration option:

* `enableConsoleLogging` — Determines whether internal Edixor logs are shown in the main Unity Console.\
  If disabled, logs will only be available in the **Debug** tab inside Edixor.

### Creating EdixorWindows

You can now proceed to [creating your custom windows](https://github.com/Edixor/edixor-documentation/tree/main/edixor)
