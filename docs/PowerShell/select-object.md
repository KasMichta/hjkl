---
layout: default
title: Select-Object Overview
parent: PowerShell
nav_order: 4
---
# Select-Object

## Basic Use

```powershell
get-childitem C:\ | select-object LastWriteTime, Name
```

## Explanation
Select-Object let's you filter what "columns" you see from the output of an expression. However, what you may not know is that often, it's already filtered.

Take the example above. This is what it usually returns:

```powershell
get-childitem C:\
```

But wait, there's more, we just need to see all of them.

### Format-List
A great cmdlet you might use all the time is Format-List.  `Format-List` quite literally does what it says on the tin, we can tell it to give us properties with the parameter `-property`, but instead of specifying one we can use the **wildcard** `*` to ask for ALL properties.

But, I rarely write it all out, there's a handy alias `fl *`, which I tend to read/imagine as "Full List". When used after a pipe, it will return ALL properties available for each object:

```powershell
get-childitem C:\ | fl *
```

Now that's a lot more. We now have the full output for each item, with new hidden properties we didn't see before.

Also be aware that you can achieve the same with `| Select *`, however I do the above, because less word do trick.

### Properties
But I don't want 100 different results, I just want to know what properties there are so I can choose which ones I want.

Luckily we have `Get-Member`, which gives us all the 'members' for an object. This also gives us a bit too much info, but we'll deal with that in a second.

```powershell
get-childitem C:\ | Get-Member
```

So that's a lot of 'members', but you'll notice the property 'MemberType' has a lot of different values. We can filter out all the ones we don't want like so:

```powershell
get-childitem C:\ | Get-Member -MemberType Properties
```

That's great, but you can shorten this down:

```powershell
get-childitem C:\ | gm -membertype prop*
```

This is the quickest way to get the properties when you're in the shell.

## Select-Object -Properties
Now we can choose the properties we want to see:

```powershell
get-childitem C:\ | Select-Object -Property Root, Name, CreationTime, LastWriteTime, Exists, Mode
```

And just in case you're asking, why is it not in a pretty table? I got you:

```powershell
get-childitem C:\ | Select-Object -Property FullName, CreationTime, LastWriteTime, Exists, Mode | ft -AutoSize -Wrap
```

`ft` being short for `Format-Table`, you don't need to use `-AutoSize` and `-Wrap`, but if your table is getting really long, they will help make it readable.

## Aliases
If you are writing a script, or any command that will need to be used more than once, I highly recommend using the full `Select-Object` cmdlet and arguments like `-Property`, or `-ExpandProperty` . But if you are just using it for the utility, you can shorten it down to `Select`, and start typing property names without any parameter prefix.

```powershell
get-childitem C:\ | Select FullName, CreationTime, LastWriteTime
```

## Changing/reformatting properties
If you want to change how the property names are displayed, their names, or even want to perform some action on the values within themselves, please refer to [Calculated Select-Object Properties]


[Calculated Select-Object Properties]: https://kasmichta.github.io/hjkl/docs/PowerShell/calculated-select.html
