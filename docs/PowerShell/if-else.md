---
layout: default
title: IF Statements
parent: PowerShell
nav_order: 7
---
# If this then that
A basic building block of most programming languages is the 'if' statement. Its general formula goes like this:

If some condition is met (it's true), do something.

In PowerShell this is achieved with this syntax:
```powershell

$IAmBusy = $true
$IAmHungry = $false

if ($IAmBusy) {
	Write-Output "I'll call you later, I see you are busy."
}

if ($IAmHungry) {
	Write-Output "Let's go get some food"
}
```

`if` checks for the expression between the `()` brackets, if it resolves to a true statement, it will run the code between the `{}` brackets.

Since the values above were defined as `$true` and `$false` directly, ([Data Structures] for more info on what those are). It's easy to see why each statement ran. 

## Let's calculate inside `()`

This time, let's calculate whether the condition is true.

```powershell

if (2+2 -eq 4) {
	Write-Output "2 plus 2 is 4!!!"
}
```

This ran the code, and the reason for that is:

```powershell
2+2 -eq 4
```

The condition outputs a `$true` or `$false` when executed. So we can only make conditions based on this logic, so we could not write:

```powershell
if (2+2 = 4) {
	Write-Output "You've done something wrong!"
}
```

Because it will fail.

So here's this:

```powershell
if (2) {
	Write-Host "Wait. What?"
}
```

I recommend looking online why this works, I only have so much free time to write these.

## How expressions inside `()` are evaluated

[Comparison and Logical Operators]

# [](#orelse)Or Else

So we can test for IF something is true, but what if it comes up false, and we want to do something **else**?

```powershell
$Colour = "blue"

if ($colour -eq "gray") {
	Write-Output "You've found my favourite colour!"
} else {
	Write-Output "No, $Colour is not my favourite colour."
}
```

Easy enough, but what if we want to do some more if checks. Since `else {}` statement will run every single time that the IF conditions comes up false, we can't really create backups. Or at least not cleanly. So for this reason we have `elseif`.

```powershell
$testResults = "We failed main check 1, but passed fallback check 2"

if ($testResults -like "*passed main check 1*") {
	Write-Output "we passed check 1!"
} elseif ($testResults -like "*passed fallback check 2*") {
	Write-Output "fallback test 2 succeeded, phew..."
} else {
	Write-Output "We're going down captain!"
}
```
However, in my personal opinion you should avoid this. If you want to your script to run a different process based on mutliple conditions, please have a look at and attempt to use switch [statements instead].

[statements instead]: https://kasmichta.github.io/hjkl/docs/PowerShell/switch-statement.html
[Data Structures]: https://kasmichta.github.io/hjkl/docs/PowerShell/data-structures.html
[Comparison and Logical Operators]: https://kasmichta.github.io/hjkl/docs/PowerShell/operators.html

