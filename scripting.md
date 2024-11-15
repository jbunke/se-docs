# Scripting

[`< Overview`](./README.md)

<!-- What? -->

## Scripting basics

Stipple Effect scripts fall into the following categories:

* [Automation scripts](#automation-scripts)
* [Preview scripts](#preview-scripts)
* [Color scripts](#color-scripts)
* [Child scripts](#child-scripts)

### Automation scripts

[Automation scripts](automation-scripts.md) are scripts that run a series of program actions automatically. For example, a script can be written that reverses the frames in a project. The full list of Stipple Effect program actions that can be executed via script are outlined in the [scripting API](../api/).

Type signature for automation scripts: `()` - (no parameters and void return)

### Preview scripts

[Preview scripts](preview-scripts.md) allow for creation of complex custom previews by transforming the flattened contents of the project that acts as input into original output. Since preview scripts are essentially a __*mapping*__, the contents of the preview window will change dynamically as the project that defines its input is modified, without having to reload the script.

Valid type signatures for preview scripts:
* `(image -> image)`
* `(image[] -> image)`
* `(image[] -> image[])`
* `(image -> image[])` - (only valid for projects with a single frame)

### Color scripts

[Color scripts](./color-scripts.md) transform an input color into an output color. They can be run independently and applied to a certain [scope](./scope.md) of the active project, or they can be used to power the [Script Brush](./scripting-brush).

Type signature for color scripts: `(color -> color)`

### Child scripts

[Child scripts](./child-scripts.md) are merely scripts that are run from within another script. Child scripts can have any type signature, but will trigger a runtime error if they are passed arguments that do not match the types of the parameters of their [header function](#syntax).

You may run scripts from within any script, but it is recommended *NOT to run child scripts from within preview or color scripts* for the sake of performance. Also in the interest of performance, it is optimal to declare any child scripts used in a script as `final` (also `~`) `script` variables outside any loops. This way, the child script will only be loaded once per its parent script's execution.

<!-- How? -->

## Getting started

Stipple Effect scripts are written in an extension dialect of [DeltaScript](https://github.com/jbunke/deltascript) designed specifically for the program. DeltaScript is a lightweight, skeletal scripting language that I designed to be extended for the easy design of [domain-specific languages](https://en.wikipedia.org/wiki/Domain-specific_language) interpreted to Java. It has a very simple syntax and will feel familiar to anyone with programming experience in languages such as C, C++, C#, Java and JavaScript.

Script files can be written in the text editor of your choosing and use the file extension `.ses`. However, `VS Code` is recommended, as the official syntax highlighting extension for Stipple Effect scripting is only available for VS Code.

### Syntax

Scripts consist of a nameless header function optionally followed by **named helper functions**. The type signature of the script is the type signature of its header function. Functions can optionally accept parameters and return a value of a specified return type, **though neither are required**.

```js
// header function - this is the entry point of the script's execution
// * has a single parameter "letters"
// * returns a string
(int letters -> string) {
    string word = "";

    for (int i = 0; i < letters; i++)
        word += random_letter();

    return word;
}

// helper function "random_letter"
// * has no parameters
// * returns a char
random_letter(-> char) {
    ~ int MIN = (int) 'a';
    ~ int MAX_EX = ((int) 'z') + 1;

    // "rand" is a native function defined by DeltaScript
    return (char) rand(MIN, MAX_EX);
}
```

A full breakdown of the syntax and semantics of DeltaScript can be found in the [language specification](https://github.com/jbunke/deltascript/blob/master/docs/lang-spec.md).

### Example

To get a feel of what DeltaScript looks like, here is an example of a preview script:

<!-- TODO - replace asset with valid DeltaScript code -->
![Script example](./assets/code-example.png)

When the script above is applied to the project on the left, it produces the preview to the right:

| Input | Output |
| :---: | :----: |
| ![Input](./assets/running.gif) | ![Output](./assets/running-channels.gif) |
| ![Input](./assets/bouncing-ball.gif) | ![Output](./assets/bouncing-ball-channels.gif) |

___

**SEE ALSO**

* [Script examples](https://github.com/jbunke/se-script-examples)
* [*DeltaScript for Stipple Effect* - VS Code syntax highlighting extension](https://marketplace.visualstudio.com/items?itemName=jordanbunke.deltascript-for-stipple-effect)
* [Stipple Effect video playlist (w/ tutorials)](https://www.youtube.com/playlist?list=PLy71S74rTLnPEwYYtAXvh2er8QBvWIwRL)
