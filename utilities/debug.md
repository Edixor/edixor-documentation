# Debug

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
