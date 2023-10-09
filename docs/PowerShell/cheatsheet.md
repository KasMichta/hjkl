---
layout: default
title: Useful Tips - Cheatsheet
parent: PowerShell
nav_order: 99
---
# Tab Completion
You may be familiar with hitting <kbd>Tab</kbd> to autocomplete cmdlets, parameters, choices when working inside the PowerShell window. But did you know you can have a more user friendly, visual output instead. 

Use <kbd>Ctrl + Space</kbd> to start the visual tab complete.

## Cmdlet completion:

![Cmdlet Completion](/hjkl/assets/images/CmdletCompletion.png)

## Parameter completion:

![Parameter Completion](/hjkl/assets/images/ParamCompletion.png)

## Parameter value completion:
For certain cmdlet parameters, you can only use specific predetermined values. Such as `Invoke-RestMethod` (used for accessing APIs). The method parameter can only have these values:

![Parameter Value Completion](/hjkl/assets/images/ParamValueCompletion.png)

# Other Useful Key Bindings

| Keys            | Function                                                   |
| --------------- | ---------------------------------------------------------- |
| `Ctrl + r`      | Search your input                                          |
| `Ctrl + a`      | Search everything in current input/command line            |
| `Shift + Enter` | Go to next line, without executing. Writing multiple lines |

# PowerShell Profile

Whenever you run a PowerShell instance, you will also be implicitly be running a profile. You can see where it is for your session, by executing `$profile`:
```powershell
$profile
```

This file can contain scripts/functions/variables that you want to have avaiable whenever you run PowerShell. It's simply a file that runs whenever you open PowerShell. You can edit it very quickly and easily with:

```powershell
notepad $profile
```

For example, if you see in the screenshots that my prompt shows `soc-hub-functions>`, rather then the usual `C:\users\...` that is because my profile contains a short function to shorten my prompt to only the folder that I am currently in. If you wanted to also have the ExchangeOnline module available whenever you start PowerShell you could include the import command in your profile like so:

![Notepad Profile](/hjkl/assets/images/NotepadProfile.png)

# Clipboard
You want to interact with the contents of your clipboard. You have both of the below available:
```powershell
Get-Clipboard
Set-Clipboard
```

These cmdlets can retrieve and set contents from/to your clipboard. I rarely use `Get-Clipboard` as `Ctrl+v` or right-clicking in the prompt does this quicker. However, setting the clipboard is extremely useful, especially with the alias `clip` . Run the below:

```powershell
$list = @('a', 'b', 'c', 'd')
$list | clip 
```

Open a notepad and paste.
Now imagine, that `$list` was much longer, or contained large amounts of data. Instead of printing into the prompt and making your scroll bar a grain of sand. Just `|clip` out the data and paste into a text editor. You can quickly prototype/diagnose issues with this.

# Aliases
Are you finding yourself using certain commands all the time, and you're tired of writing them out or writing out to the point where you can tab complete? Then set your own alias:

```powershell
Set-Alias ctj Convertto-Json
```

I convert data to JSON all the time, but I need to write `Convertto-J` every time, as there are plenty of conversion commands that will come up if I try to tab-complete early. So I can set this alias and now write `ctj` instead.

## Parameter Shorthand
In a similar vein, if you are using a parameter of a cmdlet, you don't have to write it out completely.

```powershell
Get-Process -name win* | select id, name | convertto-csv -Delimiter ';'
```

`-Delimiter` is the parameter we're looking at, I can check what parameters are available with the `Ctrl + Space` completion.

Delimiter is the only one that starts with 'd', writing 'd' will be enough for PowerShell to identify it. So we could get the same results from:

```powershell
Get-Process -name win* | select id, name | convertto-csv -d ';'
```

# Paths, Files in PowerShell
Use the alias `ii` for `Invoke-Item` to instruct PowerShell to open an item or directory. For instance, the below will open the file in the path in excel:

```powershell
ii C:\Users\...\Downloads\some_spreadsheet.xlsx
```
You can give this a go by creating a file and using `ii` followed by the full file path (including the file name and extension).

The below `ii .` will open the current folder that I am in. In this case the temp folder.

```powershell
C:\Temp> ii .
```
Running `ii .` will open whatever directory you are in, so if you use the prompt-shortening in your profile like I do, this is another way to do a quick check by opening up your current directory in file explorer.

