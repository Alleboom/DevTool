# DevTool
DevTool is a multipurpose tool to assist with World of Warcraft addon development.
The core functionality is similar to a debugger, and it is capable of visualizing and inspecting tables, events, and function calls at runtime.

This addon can help new and veteran developers alike by providing a visual representation of their tables and structures.
Examining the WoW API or your addon's variables in a table-like, columnar interface is much easier than using print(), /dump, or other chat debugging methods.

---

## How To Use
While DevTool is fully capable of being used solely from its graphical interface, it also provides a simple API for incorporation into your codebase directly.
Use of this API will result in adding elements directly into the DevTool interface for inspection.

### Using `AddData()`:

The main (and only) public function provided by DevTool is `AddData(data, <some string name>)`
- This function adds data to the list so that you can explore its values in interface list.
- The 1st parameter is the object you wish to inspect. 
  - **Note**, the default behavior is to _shallow_ copy.
- The 2nd parameter is the name string to show in interface to identify your object. 
  - **Note**, if no name is provided, we will auto-generate one for you.

Let's suppose you the following code in your addon...

```lua
local var = {}
--<some code here that adds data to 'var'>
DevTool:AddData(var, "My local var")
```

...this code will add `var` as a new row in the DevTool user interface with the label `"My local var"`.

### Example of a very common use case:
Here is a simple implementation that wraps `DevTool:AddData()` and checks for the `DEBUG` flag to be set:

```lua
function ExampleAddon:AddToInspector(data, strName)
	if DevTool and self.DEBUG then
		DevTool:AddData(data, strName)
	end
end
```

Using the above code as an example, we can then apply our new function all over the addon codebase wherever inspection is needed:

```lua
ExampleAddon:AddToInspector(ExampleObject, "ExampleObjectName")
```

### How to use the sidebar:
There are 3 tabs in sidebar and text field has different behavior in each tab.

- History tab:
	- This is just an easy way to call `/dev ...` for example you can print `find DevTool` and it is the same as printing `/dev find DevTool` in chat.
- Events tab:
	- This text field can only use `eventname` or `eventname unit` and this is the same as `/dev eventadd eventname` or `/dev eventadd unit` where `eventname` is a [Blizzard API event](https://wowpedia.fandom.com/wiki/Events) string name.
- Fn Call Log tab:
	- You can type `tableName functionName` into the text field, and it will try to find `_G.tableName.functionName`.


### How to use function arguments:
You can specify coma separated arguments that will be passed to the function. The values can be in the form of a `string`, `number`, `nil`, `boolean`, and/or `table`.
- **Note**, _to pass a value with type `table` you have to specify prefix `t=`_.

Example passing arguments to a function `SomeFunction`:
- FN Call Args: `t=MyObject, 12, a12` becomes `SomeFunction(_G.MyObject, 12, a12)`
- FN Call Args: `t=MyObject.Frame1.Frame2` becomes `SomeFunction(_G.MyObject.Frame1.Frame2)`

### Chat commands:
- `/dev` - toggles the main UI window
- `/dev help` - Lists help actions in the chat window
- `/dev <command>` - Will execute one of the commands listed in the help menu

### Other functionality:
- Clicking on a table name will expand and show its children.
- Clicking on a function name will try to call the function. **WARNING: BE CAREFUL**.
- If a table has WoW API `GetObjectType()` then its type will be visible in the value column.
- DevTool can monitor WoW API events similar to that of `/etrace`.
- DevTool can log function calls, their input args, and return values.
	- **Note**: Strings in the 'value' column have no line breaks

---

## Want to contribute?
* [Report Bugs and Request Features](https://github.com/brittyazel/DevTool/issues)
* [Source Code](https://github.com/brittyazel/DevTool)

---

## Want to Donate?
Making add-ons is a lot of work! Your help goes a huge way to making my add-on work possible. If you would like to Donate, [GitHub Sponsors](https://github.com/sponsors/brittyazel) is the preferred method.

---

## Credits:
DevTool is a continuation of the amazing [*ViragDevTool*](https://github.com/varren/ViragDevTool) addon started by Varren/Virag for World of Warcraft: Battle for Azeroth and prior. All credit for the idea and the work done prior to 2021 should go to him, accordingly.
