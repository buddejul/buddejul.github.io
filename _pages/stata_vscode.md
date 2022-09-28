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
While coding I regularly find myself repurposing code blocks from old code, these might be simple things like a routine to calculate within-group maxima or more complex things like summary tables or collapse statements. 
One feature of VS Code I find particularly helpful is the ability to use pre-written Code snippets in an intuitive way. What is best, these snippet are easy to write on your own. 


How does it work?
- VS Code has its own "snippet" feature with a simple language that allows highly customizable yet easy to write snippets.
- You can see your current snippets the following way:
    - `ctrl + alt + p` to open your search bar, enter "snippets" and hit "Configure user snippets"
    - Further down below your existing snippets clicking the option "New global snippets file..." will give you a snippet template for a snippet.
    - Snippets follow a specific language that defines things like their main body, their intended languages, or the word they are called by.
- Once you have configured a snippet you can just invoke it by typing the designated prefix into your code and hitting enter.


## Examples
A more comprehensive guide to writing snippets can be found here: [https://code.visualstudio.com/docs/editor/userdefinedsnippets](Snippets in Visual Studio Code
). I will demonstrate its usefulness with two examples in the following sections.
### Variables by Group
Often in panel data settings you want to generate variables that are aggregated on some level.
For example, in an individual-establishment-year panel with wage information you might want to create a variable that stores the maximum wage within each firm and year.
One way of doing this without altering the structure of the data at hand (e.g. by collapsing) is using a command like `egen max_wage = max(wage), by(establishment year)`.
However, especially `egen` commands tend to be (a lot) slower than hard-coded commands and faster options like `gegen` might not always be available.
An alternative way of finding the maximum of a variable is 
```Stata
gen max_wage = wage
bysort establishment year: replace max_wage = max(max_wage[_n-1], max_wage)
bysort establishment year: replace max_wage = max_wage[_N]
```
which can be hard to remember. At the same time you might not want to put this in a separate ado file because that can defeat the purpose of having a hard-coded command based on Stata primitives.

Now this is where a snippet comes in handy. The following snippet creates a template for the above code:
```
{
	// Code for the maximum in a group (panel data)
    "Stata max by group": {
		"scope": "stata",
		"prefix": "groupmax",
		"body": [
            "gen max_${1:var} = ${1:var}",
            "bysort ${2:group}: replace max_${1:var} = max(max_${1:var}[_n-1], max_${1:var})",
            "bysort ${2:group}: replace max_${1:var} = max_${1:var}[_N]"
		],
		"description": "Maximum in a group"
	}
}
```
You can see the corresponding code in the body which is put in the editor upon typing groupmax and hitting enter.
However, notice the "$" decorators that act like variables: The above routine has two inputs (wage and the by-group) and the way the snippet is coded you only need to type them *once*, while also giving you a consistent naming of variables.

This is most easily demonstrated by a short video:

<video src="videos/snippets_groupvars.mp4" data-canonical-src="videos/snippets_groupvars.mp4" controls="controls" muted="muted" class="d-block rounded-bottom-2 width-fit" style="max-height:640px;">
</video>

### Binscatter Routine

## Implementations

## Todo

- Hard-coded equivalents to egen/gegen (groupmax, groupmin, grouptotal)
    - These require no packages and are faster than egen, but usually slower than gegen
- Esttab templates (esttab_regtab)


# Other Editors
Atom has a package by Kyle Barron that allows not only for Syntax highlighting but *in-editor* results: https://kylebarron.dev/stata_kernel/
But: Interactive coding is still possible in a Jupyter Notebook using VSCode (Q: Is there a package that allows for integration into plain code?)