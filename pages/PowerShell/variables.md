---
layout: default
title: Variables
parent: PowerShell
nav_order: 1
---

# $Variable
All variables in PowerShell start with a `$` symbol

A variable is a place to store data. It's a box with a label on it. Let's say you have a list of 400 users with their names, email addresses, job titles etc. You can just put it in a variable called `$userlist`, and then just use the variable name when you need to reference it, or perform some process with it.
 
```powershell
#Putting something in a variable
$variable = 50

#Checking what is in a variable
$variable
```

There's a slightly longer way to do this, using [[Cmdlets]] for a declaring variables:

```powershell
Set-Variable -Name Colour -Value "blue"

Get-Variable -Name Colour
```

You'll notice that this way you don't have to use the `$` to set or read the variable.

# Pre-defined Variables
PowerShell does have some variables that it itself uses while running. Some of these will be session-based, and some will be based on the environment you are in.

Here are some examples.

Version information for the shell itself:
```powershell
$PSVersiontable
```

# Nothing
The `$null` variable represents nothing, assigning `$null` to something will let you create a variable that does not store anything.

```powershell
$thisIsNothing = $null

$thisIsNothing
```

No Output, because no thing.

It is highly useful for checking if a variable is empty ([[IF Statements]]):

```powershell
Set-Variable -Name empty

$notEmpty = 4

if ($empty -eq $null){
	Write-Output '$empty is null'
}

if ($notempty -eq $null){
	Write-Output '$notEmpty is null'
}
```

# True or False
Another pre-set couple of variables are `$true` and `$false` you cannot change these, these are used to represent a single-bit value, i.e. a 1 or 0 in memory. 
They are called Booleans because of a guy called George Boole, more on these in [[Data Structures]].

```powershell
$Mary = @{
	hasAnOrange = $true
	hasABanana = $false
}

if ($Mary.hasAnOrange) {
	Write-Output "Mary has an orange."
}

if ($Mary.hasABanana) {
	Write-Output "Mary has a Banana."
}
```

One value is true, the other is set to false, only the true one will run. 

# Variables inside variables
Don't exist.

By equating one variable to another, you will simply copy the data from the variable on the right side of the `=` to the left.

```powershell
$variable_one = 10

$variable_two = $variable_one

$variable_two
```

