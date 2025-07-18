# Lifecycle

## EdixorTab Lifecycle

An `EdixorTab` goes through a well-defined series of steps from creation to destruction. Below is an overview of each stage and the methods invoked:

***

### 1. Creation & Registration

* **ShowTab\<T>()**\
  Static helper that:
  1. Loads the `DIContainer`
  2. Resolves the `ITabController`
  3. Calls `AddTab(…)` on the controller, causing:
     * A new `TabData` asset to be created via `CreateTabData(type)`
     * The tab to be persisted and scheduled for initialization

***

### 2. Metadata Preparation (Without UI)

* **PrepareMetaDataWithoutUI()**
  * Calls `SetupLifecycleDelegates()`, wiring up any overrides of `Awake`, `Start`, etc.
  * Immediately invokes the child’s `Awake` callback (so metadata like `tabName` and counts are set).

***

### 3. Data-Driven Construction

* **CreateTabData(Type logicType)**
  * Instantiates a bare `EdixorTab` of that type
  * Calls `LoadUxml("auto")`, `LoadUss("auto")`, `LoadIcon(null)` to discover asset paths
  * Packages paths and icon into a new `TabData` ScriptableObject

***

### 4. Initialization with UI

* **Initialize(TabData data, DIContainer cont, VisualElement parent)**
  1. Deserializes fields from `TabData` (title, icon, UXML/USS)
  2. Clones the UXML tree into `root` and adds stylesheets
  3. Clears the `parentContainer` and adds `root`
  4. Sets up `container`
  5. Calls `SetupLifecycleDelegates()`

***

### 5. Awake & Start

* **InvokeAwake()**
  * Clears and re-adds `root` to the container
  * Calls your override of `Awake()` (if implemented)
* **InvokeStart()**
  * Immediately after `Awake`, calls your `Start()` override (if any)

***

### 6. Enabling & Disabling

* **InvokeOnEnable()**
  * Clears and re-adds `root`
  * Calls your `OnEnable()` override
  * Applies styles via `InitStyleTab(...)`
* **InvokeOnDisable()**
  * Calls your `OnDisable()` override

***

### 7. Update Loop

* **InvokeUpdateGUI()**
  * Called once per frame (or editor tick) if your tab is active
  * Invokes your `UpdateGUI()` override

***

### 8. Destroy & Cleanup

* **InvokeOnDestroy()**
  * Calls your `OnDestroy()` override
  * Sets `root = null` to release UI
* **DeleteUI()**
  * Alias for cleanup: invokes destroy and nulls `root`

***

### 9. Usage in TabController

* **CreateLogic(TabData)**
  * Instantiates the `EdixorTab`, calls `Initialize(data,…)`, then:
    * `InvokeAwake()`
    * `InvokeStart()`
* **SwitchTab(index)**
  * For the old tab: `InvokeOnDisable()`
  * For the new tab:
    * Updates CSS classes
    * Clears content, adds new `root`
    * Calls `InvokeOnEnable()`
* **CloseTab(index)**
  * `InvokeOnDisable()` → `InvokeOnDestroy()` → remove UI and asset

***

By following this sequence, Edixor ensures that each tab’s UI and logic are properly constructed, styled, and cleaned up in a consistent, Unity-style lifecycle.
