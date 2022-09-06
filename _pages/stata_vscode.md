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
TEST Provided by Kyle Barron, Stata Enhanced provides "syntax highlighting in Visual Studio Code, built from the ground up".
The extension highlights almost everything in a nice and readable manner.

### Runing Stata from VS Code: stataRun, Code Runner, ...
(Note: This is specific to Windows; other solutions might be available for other operating systems, e.g. Linux, as described in the Stata Enhanced readme.)

- https://blog.karatos.in/a?ID=01700-42564408-6082-4731-87b4-d40f85f9474f
- links to this: https://huebler.blogspot.com/2008/04/stata.html

# Other Extensions

# Snippets
- Hard-coded equivalents to egen/gegen (groupmax, groupmin, grouptotal)
    - These require no packages and are faster than egen, but usually slower than gegen
- Esttab templates (esttab_regtab)


# Other Editors
Atom has a package by Kyle Barron that allows not only for Syntax highlighting but *in-editor* results: https://kylebarron.dev/stata_kernel/
But: Interactive coding is still possible in a Jupyter Notebook using VSCode (Q: Is there a package that allows for integration into plain code?)