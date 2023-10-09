---
layout: default
title: For Each - Looping
parent: PowerShell
nav_order: 9
---
# For Each

How to do something to every item in a collection of things.
This is where the automation really kicks in.

## Basic Usage

```powershell
$trees = @(
	@{name="Alder";family="Betulaceae"},
	@{name="Birch";family="Betulaceae"},
	@{name="Beech";family="Fagaceae"},
	@{name="Walnut";family="Juglandaceae"},
	@{name="Ash";family="Oleaceae"} 
)

foreach ($tree in $trees) {
	$tree.name + " is in the " + $tree.family + " family."
}
```

## Structure of foreach

The foreach function is great for looping through items, it follows this beautifully illustrated structure:

![ForEach](/hjkl/assets/images/ForEach.png)

So in the above example:
	First Loop: `$item` is `$something[0]`, do something with value `x`, if `[0]` looks new to you refer to [Indexing]
	Second Loop: `$item` is `$something[1]`, do something with value `y`.
	Third Loop: `$item` is `$something[2]`, do something with value `z`.
	... 

This is great in scripts and can be used for almost anything, sometimes you might want to end the loop early:

## Continue, Break

For Each has a couple key words that it can use to dictate whether it can keep looping or stop early.

`continue` means go to the next loop please, for example:

```powershell
foreach ($num in 1,2,3,4,5) {  
  if ($num -eq 2) { continue }
  $num  
 }
```

This skips the number 2, because we told it to `continue` (to the next loop) before it could get to the `$num` call that would display the number.

`break` means cut the loops off completely, if a foreach runs into a `break` word is will not run any further loops at all.

```powershell
foreach ($num in 1,2,3,4,5,6,7) {
	if ($num -eq 5) { break }
	$num
}
```

Again, the loop did not display `5` as an output because the `break` clause was reached before the `$num` call to print the number.

## ForEach-Object

Foreach works great for writing scripts, its great strength is readability as you can set the iterator to a sensible name like `$item` or `$tree`. However, we often just want to use foreach on the fly, sometimes even on the same line.

For this purpose we use `ForEach-Object` which is used in the pipeline (below we use shorthand `ForEach`):

```powershell
get-service -DisplayName *update* | ForEach {'Service "{0}" is {1}' -f $_.DisplayName, $_.Status}
```

In one line, we can perform an action for each object generated from the left side of the `|`. 
The only thing that differs, is having to use the `$_` variable. 

[The Pipeline Variable _]

### The shorthand for the shorthand

Even further than the above, we can completely avoid using the foreach keyword and replace it with `%`.

Here's the same command as above but with the shortest foreach declaration:

```powershell
get-service -DisplayName *update* | % {'Service "{0}" is {1}' -f $_.DisplayName, $_.Status}
```

## The foreach method

A similar approach to the `.where()` method mentioned in [Where] there is a `.foreach()` method following the same structure.

```powershell
(Get-Service r*).foreach({'{0}--{1}' -f $_.name, $_.status})
```

This is still very useful, but can reduce readability when writing very long lines of code. The `|` pipeline is one of the core functions of PowerShell and has more troubleshooting resources online, which is why it is often preferred.

[Indexing]: https://kasmichta.github.io/hjkl/docs/PowerShell/data-structures.html#indexing
[The Pipeline Variable _]
[Where]: https://kasmichta.github.io/hjkl/docs/PowerShell/where-object.html#where

