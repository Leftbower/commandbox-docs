# Tab Completion & Help

Tab completion and help are powered by metadata on your command CFCs. For basic command and parameter tab completion to work you don't need to do anything. CommandBox will look at the metadata of your command and just use it.

## Command Help

When a user types `yourCommand help`, the text they see is the hint attribute on the component. The easiest way to add this is via a Javadoc-style comment at the top of your component. Feel free to include sample execuctions which can be wrapped in `{code:bash}{code}` blocks.

```javascript
/**
* This is a description of what this command does!
* This is how you call it:
*
* {code:bash}
* myCommand myParam
* {code} 
* 
**/
component {
    ...
}
```

## Parameter Help

You'll also want to provide hints for each of the parameters to the `run()` method. This will also be visible in the help command as well as the Command API docs.

```javascript
/**
* @param1.hint Description of the first parameter
* @param2.hint Description of the second parameter
*/
function run( required String param1, Boolean param2 ) {
    ...
}
```

## Parameter Value Completion

When a user hits tab while typing a parameter value, booleans will prompt with true/false. Parameters with `directory`, `destination`, `file`, or `path` in their names will automatically give file system suggestions.

You can provide additional hints though for your parameters to make them super user friendly.

### Static Options

If a parameter has a set number of exact options, you can specify an `options` attribute for that parameter that is a comma-delimited list of options for tab completion to give.

```javascript
/**
* @gender.hint Please tell us your gender
* @gender.options Male,Female,Unknown
*/
function run( String gender ) {
    ...
}
```

### Options UDF

If a parameter's values are dynamic, you can specify a `optionsUDF` attribute for that parameters that points to a function name in the command CFC that will be called. This function needs to return an array of all possible values. CommandBox will do the work of filtering down partial matches if they're in the middle of typing.

```javascript
/**
* @forgeBoxType.hint What type of package is this?
* @forgeBoxType.optionsUDF completeTypes
*/
function run( String forgeBoxType ) {
    ...
}

array function completeTypes() {
    // Return array of possible types
}
```

An example of this in action is the install command. It will auto-complete the ForgeBox slug for you. Try typing `install cold` and hitting tab to see what happens.

## Namespace help

If you have a namespace \(folder\) of commands, you can control what the user sees when they type `namespace help` by creating a command CFC called `help.cfc` in the root of that folder. The help command should print out whatever information you want. CommandBox will automatically call it when the users needs help for that namespace.
