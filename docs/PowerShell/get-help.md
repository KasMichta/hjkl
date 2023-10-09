---
layout: default
title: Get-Help
parent: PowerShell
nav_order: 5
---
# Get-Help

When you are ever unsure about:
- What a command does
- How to use it

Get-Help is your one-stop shop, just use `Get-Help <Command>`.
Or is it?

Well there's only one way to find out:

```powershell
Get-Help Get-Help
```

So you'll notice the help is split into several sections, not every single help article will have all of them, but most commands generally follow this standard:

## Name
yep.

## Synopsis
Short function description

## [](#syntax)Syntax
This is where you will need to get used to syntax notation, which is fairly easy to grasp once you do it once or twice. 

### Syntax Sets
You'll see there are 6 different variations of syntax, these are called syntax sets. In short, these are different ways to use a command based on what you need to use it for. Here's a simple example:

```powershell
Get-Help Get-Item
```

There are only two sets:
```powershell
Get-Item [-Credential ] [-Exclude ] [-Filter ] [-Force] [-Include ] -LiteralPath [-Stream ] [-UseTransaction] [] 
Get-Item [-Path] [-Credential ] [-Exclude ] [-Filter ] [-Force] [-Include ] [-Stream ] [-UseTransaction] []
```

One has everything in `[]` the other has `-LiteralPath` not in brackets. The `[]` will be explained in the next section in more depth, but in essence, if you are using `Get-Item -LiteralPath` you are using the first set, and you can only use the parameters (`-parameters`) of that set; you would not be able to use `-Path` because that only applies to the second set. 

This allows for versatility in commands, they can "do different things", but won't let you mix up that functionality. Here's what would happen if I tried to use both.

```powershell
Get-Item -Path .\ -LiteralPath $env:USERPROFILE\AppData\local\Obsidian
```

An Error occurs reading `Get-Item : Parameter set cannot be resolved using the specified named parameters.` because there is no parameter that contains both `-Path`, and `-LiteralPath`. That makes sense, why would we want to supply a relative path like `.\` (which means: current working directory) and also a full literal path, the command is written in a way so that functionality is separated to avoid mistakes (we may accidentally enter two different paths).

### So what do `[]` mean in Syntax sets

Here's the third set of syntax from the `Get-Help Get-Help` command:

```powershell
Get-Help [[-Name] ] [-Category {Alias | Cmdlet | Provider | General | FAQ | Glossary | HelpFile | ScriptCommand | Function | Filter | ExternalScript | All | DefaultHelp | Workflow | DscResource | Class | Configuration}] [-Component ] [-Full] [-Functionality ] [-Path ] [-Role ] []
```

Firstly, every `[]` means that, whatever is between the brackets is optional. I will now take away anything that is in brackets:

```powershell
Get-Help
```

So Get-Help can be ran without anything else. When you run this you'll notice you'll get an introduction to the help system. Which I could have shown you from the start, but I just like being difficult.

So let's look at just the first very first set of brackets from the syntax set I chose earlier:

```powershell
Get-Help [[-Name] ]
```

The outer brackets mean that it's optional to have `[-Name]` in the- wait, but that's in `[]` too, so it's optional twice?

### What do nested `[]` mean

It's a little confusing at the start, but what it boils down to is:

Let's say we want help for `get-service`:

```powershell
Get-Help -Name Get-Service
```

Well now let's put the brackets we saw in the syntax back in for demonstration:

```powershell
Get-Help [[-Name] Get-Service]
```

So:
- `[[-Name] Get-Service]` is optional, that makes sense, because without it we'd just be running `Get-Help` and that's valid.
- `[-Name]` is optional, because just like in the very first command of this document we can use:

```powershell
Get-Help Get-Service
```

So what nested `[]` mean is that we don't always need to include a `-parameter` to use the parameter. So we don't need to put `-Name` when we specify a command.

Here's a different command that also has a similar syntax example:

```powershell
Get-Help Get-Process
```

The very first Syntax set has `[[-Name] ]` as well.
Which is what allows us to search for a process like this:

```powershell
Get-Process task*
```

Rather than having to type out:

```powershell
Get-Process -Name task*
```

### The stuff that's not in `[]`

Let's look at the second syntax set of `Get-Help Get-Help`

```powershell
Get-Help [[-Name] ] [-Category {Alias | Cmdlet | Provider | General | FAQ | Glossary | HelpFile | ScriptCommand | Function | Filter | ExternalScript | All | DefaultHelp | Workflow | DscResource | Class | Configuration}] [-Component ] -Examples [-Functionality ] [-Path ] [-Role ] []
```

Now when I remove all the `[]` we get:

```powershell
Get-Help -Examples
```

This set is saying, if you want to use this set, you **must** use -Examples.

Let's test it out for `Get-Process`:
```powershell
Get-Help Get-Process -Examples
```

This gives us a LOT of information for examples, it gives sample commands, their function and explanation. A fantastic way to get started with any new command is to look up the examples. Built-In PowerShell commands usually have extensive information like this. If you download additional PowerShell modules, the amount of Get-help information will depend on how much effort the author put in. (Sometimes really good, sometimes abhorrent).

### Parameter Data Types
When looking at parameter sets, sometimes we even get information on what data the parameter is expecting. Please run the below in your own PowerShell window (Obsidian doesn't display some important info for this.)

```powershell
Get-Help Get-item
```

The first syntax set looks like this:

```powershell
Get-Item [-Credential <System.Management.Automation.PSCredential>] [-Exclude <System.String[]>] [-Filter
    <System.String>] [-Force] [-Include <System.String[]>] -LiteralPath <System.String[]> [-Stream <System.String[]>]
    [-UseTransaction] [<CommonParameters>]
```

Again, removing all the optional parameters we get:

```powershell
Get-Item -LiteralPath <System.String[]>
```

We get the parameter argument `-LiteralPath`, followed by by a Data Type (If you want more on this refer to [Data Structures]). Between the `<>` we get a type of data, in this case a `System.String[]` ([System.String addenum]).
So `-LiteralPath` is expecting a String-type value to follow.

If we look at the `-Credential` parameter, it's expecting `<System.Management.Automation.PSCredential>`, so if we want to supply a credential, we'll need to research how to create this type of data first.

#### What if there is no type after a `-parameter`
If you see a parameter set that has a parameter that's not followed by any data type like in the case of [[#The stuff that's not in]], the `-examples` parameter, this means the parameter is a "switch". And like a light switch, it can either be on or off. If it's not used, it's in the off state, but when you add it in it is switched on:

Off:
```powershell
Get-Help Get-Process
```

On:
```powershell
Get-Help Get-Process -Examples
```

This type of parameter can also be overridden, sometimes commands will work with a "reverse switch parameter", meaning it's always on. So how would we turn that off?

##### Switch Parameter Override
Some commands do scare stuff and will ask for confirmation like this:

```
Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
```

For instance a command for deleting SharePoint resources:
```powershell
Remove-SPWebApplication $mainurl -DeleteIISSite -RemoveContentDatabases
```

This is a very valuable feature to avoid mistakes when performing High-Impact changes. But if are bulk deleting things with a script, we don't want to manually approve 1000s of deletions, in this case we need to prevent this confirmation prompt. 
However, we can't just include `-confirm` because it's already on. So we negate it:

```powershell
Remove-SPWebApplication $mainurl -DeleteIISSite -RemoveContentDatabases -confirm:$false
```

We have told the command explicitly that we want the `-confirm` switch off. Very rarely, you may need to do the opposite, but just be aware that this is how `-switch` parameters are manipulated:

```powershell
Some-Command -switch:$true
```

## Get the Online Docs

Maybe you want to look up the command online, go through the MS Docs and you find that approach easier. That's just as good, if not, better.

So instead of opening up your browser and looking up a command, just add `-online`:
```powershell
Get-Help Get-Process -Online
```

## Get Examples

Like mentioned earlier in this document, add `-examples` to see examples along with some explanations:
```powershell
Get-Help Get-Process -Examples
```

## Get help for a specific parameter

Let's say you know the command, but you just want some help understanding a parameter that you want to use:

We know `Get-Process` has an `-id` parameter, so let's find out more about it, when you look it up notice that you don't need the preceding `-`: 

```powershell
Get-Help Get-Process -Parameter id
```

Or maybe the `-credential` parameter for `Get-Item`:

```powershell
Get-Help Get-Item -parameter credential
```
[Data Structures]
[System.String addenum]
