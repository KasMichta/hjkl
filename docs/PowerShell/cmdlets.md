---
layout: default
title: Cmdlets
parent: PowerShell
nav_order: 2
---
# Cmdlets
Cmdlets (Command-lets) are a core concept for PowerShell scripting. They are the most common utility when using PowerShell, and you will likely have already used them. What's the difference between Commands and Cmdlets? Mainly that Cmdlets aren't written in PowerShell, they are utilities written in other languages that are then used in PowerShell.

Here are some examples of Cmdlets:
```powershell
Import-Module ExchangeOnline

Connect-ExchangeOnline

Get-Mailbox -identity "user@company.com"

Get-Service

Get-Process
```

The core idea is "Do-Something", a verb followed by the resource that the action affects.

A lot of the time there are aliases for the cmdlets that lets you access their function quickly. A common one used often is the `fl` after a `|` Pipe to fully list the contents of a query.

```powershell
Get-Process -name "audiodg" | fl
```

`fl` is just an alias for `Format-List`

Same for `ft` which is short for `Format-Table`, you can see that both of the below return the same results:

```powershell
Get-Process -name "audiodg" | ft
```

```powershell
Get-Process -name "audiodg" | Format-Table
```

Not all cmdlets have aliases, but if you ever want to check if any exist use [Get-Help] which has a section specifically for listing aliases. Below is the output of `Get-Help Get-Member`: 

```powershell
NAME
    Get-Member
    
SYNTAX
    Get-Member [[-Name] <string[]>] [-InputObject <psobject>] [-MemberType {AliasProperty | CodeProperty | Property | NoteProperty | ScriptProperty | PropertySet | Method | CodeMethod | ScriptMethod | Methods | ParameterizedProperty | MemberSet | Event | Dynamic | InferredProperty | Properties | All}] [-View {Extended | Adapted | Base | All}] [-Static] [-Force] [<CommonParameters>]
    
ALIASES
    gm
    
REMARKS
    Get-Help cannot find the Help files for this cmdlet on this computer. It is displaying only partial help.
        -- To download and install Help files for the module that includes this cmdlet, use Update-Help.
        -- To view the Help topic for this cmdlet online, type: "Get-Help Get-Member -Online" or
           go to https://go.microsoft.com/fwlink/?LinkID=2096704.
```

[Get-Help]: https://kasmichta.github.io/hjkl/docs/PowerShell/get-help.html
