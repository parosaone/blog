---
layout: post
title: Visual Studio Code Keyboard Shortcuts Favorites
date: 2020-09-16 12:00:00 
lastmod : 2020-10-30 13:40:00 
---

Personal note on [Visual Studio Code](https://code.visualstudio.com/)'s keyboard shortcuts in Windows Operating System. Some shortcuts are added at later versions, so update VS Code if needed.

## Command Palette
`Ctrl+Shift+P`

No need to memorize every single keyboard shortcuts. (e.g. In [Markdown All in One](https://github.com/yzhang-gh/vscode-markdown), 'Open Preview' command can be accessed by `Ctrl+Shift+P` and then selecting `>Markdown: Open Preview`. Do not have to memorize the specific shortcut `Ctrl+Shift+V`.)

![Command Palette]({{site.url}}/static/2020-9-16-visual-studio-code-keyboard-shortcuts-favorites/01.PNG)

## Comment
Can be applied to individual line or block(lines). Adapts to the selected language. (e.g. If the 'Select Language Mode' is set to HTML, the comment is added by `<!-- -->`. Do note that if the target line(s) is inside the `<script>` tag, the comment is added by `//`.)

### Comment Add
`Ctrl+K` & `Ctrl+C` 

### Comment Remove
`Ctrl+K` & `Ctrl+U` 

## Define Regions

> You can fold regions of source code using the folding icons on the gutter between line numbers and line start. Move the mouse over the gutter and click to fold and unfold regions.

> Regions can also be defined by markers defined by each language. The following languages currently have markers defined:

|Language|Start region|End region|
|--- |--- |--- |
|Bat|::#region or REM #region|::#endregion or REM #endregion|
|C#|#region|#endregion|
|C/C++|#pragma region|#pragma endregion|
|CSS/Less/SCSS|/*#region*/|/*#endregion*/|
|Coffeescript|#region|#endregion|
|F#|//#region or (#_region)|//#endregion or (#_endregion)|
|Java|//#region or //<editor-fold>|// #endregion or //</editor-fold>|
|Markdown|<!-- #region -->|<!-- #endregion -->|
|Perl5|#region or =pod|#endregion or =cut|
|PHP|#region|#endregion|
|PowerShell|#region|#endregion|
|Python|#region or # region|#endregion or # endregion|
|TypeScript/JavaScript|//#region|//#endregion|
|Visual Basic|#Region|#End Region|

## Multi-Cursor

### Insert Cursor Directly Above(Below)
`Ctrl+Alt+↑/↓`

![Multi-Cursor](https://code.visualstudio.com/assets/docs/editor/codebasics/multicursor.gif)

### Insert Cursor Infront of Specific Word
`Ctrl+D`
> Ctrl+D selects the word at the cursor, or the next occurrence of the current selection.

![Multi-Cursor for Word](https://code.visualstudio.com/assets/docs/editor/codebasics/multicursor-word.gif)

### Open Folder with Code
Not a keyboard shortcut.

Inside Windows Explorer, right-click on a (1) folder icon (e.g. navigation pane) or (2) an empty background space. Click the 'Open with Code' context menu.

!['Open with Code' Context Menu]({{site.url}}/static/2020-9-16-visual-studio-code-keyboard-shortcuts-favorites/02.PNG)

Requires the following options to be enabled at setup.

![Visual Studio Code Installation Options](https://user-images.githubusercontent.com/9638156/48968771-6f715080-f005-11e8-8486-220cc183a830.png)


## Source

* [Visual Studio Code Basic Editing](https://code.visualstudio.com/docs/editor/codebasics)
* [Visual Studio Code Tips and Tricks](https://code.visualstudio.com/docs/getstarted/tips-and-tricks)
* [Visual Studio Code Keyboard shortcuts for Windows](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)