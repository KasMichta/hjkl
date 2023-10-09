---
layout: default
title: PSCustomObject
parent: PowerShell
nav_order: 95
---

# PSCustomObject
This creates a custom object with properties, useful if using multiple sources of data to create a new type of record.

```run-powershell
$myObject = [PSCustomObject]@{
    Name     = 'Kas'
    Language = 'PowerShell'
    State    = 'Powering Through'
}

$myObject
```

## Converting a Hashtable to an object

```run-powershell
$myHashtable = [ordered]@{
    Users    = 'Mothers'
	State    = 'Single'
	Location = 'Near You'
}
$myObject = [pscustomobject]$myHashtable

$myObject
```

