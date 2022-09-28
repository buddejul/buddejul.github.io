---
permalink: /stata_vscode/
title: "Stata in VS Code"
author_profile: true
# redirect_from: 
  # - /stata_vscode/
  # - /stata_vscode.html
---

A guide that describes how to use Stata in VS Code (under Windows).

# VS Code
General guide on using VS Code: https://aeturrell.github.io/coding-for-economists/code-preliminaries.html
# Setup
First download and install the latest version of VS Code.

## (Stata) Extensions
One of the great things about VS Code are user contributed extensions. 
There are few - though not many - useful VS Code extensions for Stata as well. These allow to (a) use Stata syntax highlighting and (b) run Stata 
Furthermore, it is possible to prepare blank code snippets for Stata specific uses (more on the see below).

In general, to install extensions click on the Extensions icon in the VS Code sidebar (the one with the three + one blocks).
### Stata Enhanced
Provided by Kyle Barron, Stata Enhanced provides "syntax highlighting in Visual Studio Code, built from the ground up".
The extension highlights almost everything in a nice and readable manner.

## Setup VS Code: How get Stata to run.

Getting Stata to run from within VS Code relies on two things: `Code Runner`, a VS Code extension that allows running code (for all languages) from within VS Code, and `rundo(lines).exe`, a user-written command that implements a code-running script for Stata.

All credits to Friedrich Huebler for writing the script as well as for the Blog entry on the [Karatos Blog](https://blog.karatos.in/a?ID=01700-42564408-6082-4731-87b4-d40f85f9474f), which this guide basically follows one-to-one.

- Step 1: Set up the `rundo(lines).exe` scripts. These are scripts that could in principle be called from any advanced text editor and that do nothing else than putting any code in a temporary file, opening stata, and copying the code into the Stata command window. `rundo.exe` exectues whole do files, while `rundolines.exe` allows to execute only sections.
    - A detailed guide can be found here: [https://huebler.blogspot.com/2008/04/stata.html](https://huebler.blogspot.com/2008/04/stata.html)
    - Only follow *Step 1* and *2* in the setup section, we will do the rest below.
    - NB: Section 5 contains many hints for trouble shooting.
    - For the settings I use see the attached `rundo(lines).ini` file (note paths and maybe Stata window names need to be changed either way).
- Step 2: Install the `Code Runner` extension in VS Code.
    - In the left bar go to the extensions section (the building blocks).
    - Search for `Code Runner` and install the extension.
- Step 3: Linking `Code Runner` with the scripts.
    - After installing the extension, go to the extension page and click on the settings icon next to the Uninstall button and open the “*Extension SettingsI”.*
    - In the extension settings you need to edit several points:
        - `Custom Command`: Put in the path to `rundolines.exe`
        - `Executor Map By File Extension`: Click *Edit in settings.json* which will open your personal VS Code settings file.  Here you should add the following work space settings (your customCommand should already be there):
        
        ```json
        "code-runner.executorMapByFileExtension": {
            ".do": "[path]\\rundo.exe"
        },
        "code-runner.customCommand": "[path]\\rundolines.exe",
        ```
        
        - Save the file and exit the settings page(s).
    - Now the only thing left to do is setting the keyboard shortcuts:
        - Go to the VS Code settings (the cog in the buttom left) and open “*Keyboard Shortcuts*”
        - Search for “*code-runner.*” which gives all keyboard shortcuts associated with the extension.
        - For the execution of whole do files look for `code_runner.run` and click on the edit symbol (the pen) to enter your desired keyboard shortcut.
        - For the execution of highlighted lines only look for `code_runner.runCustomCommand`.
        - NB: FH Blog *Known problem 2* suggests to not use either the “ctrl [strg]” or “shift” keys.
    - Now you should be able to run Stata from within VS Code by either using your shortcut to run whole do files or to run highlighted lines. This should work with both Stata already open and Stata initially closed.

### Bugs

I occasionally run into a bug where code is not executed and I cannot use the keyboard or only in an altered way (stopped by calling `ctrl + alt + del`). This might have been the result of they stuck key issue described in *Known problem* 2 when using `ctrl` as part of the keyboard shortcut.

# Other Extensions

# Snippets
- Hard-coded equivalents to egen/gegen (groupmax, groupmin, grouptotal)
    - These require no packages and are faster than egen, but usually slower than gegen
- Esttab templates (esttab_regtab)


# Other Editors
Atom has a package by Kyle Barron that allows not only for Syntax highlighting but *in-editor* results: https://kylebarron.dev/stata_kernel/
But: Interactive coding is still possible in a Jupyter Notebook using VSCode (Q: Is there a package that allows for integration into plain code?)