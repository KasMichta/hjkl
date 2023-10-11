---
layout: default
title: Out-Null
parent: PowerShell
nav_order: 93
---
# Out-Null/\[Void\]
When a command is producing output and for whatever reason you do not want this output, you can pipe into `Out-Null` which will supress output.

Example:
```powershell
$arraylist = New-Object System.Collections.ArrayList

$arraylist.add('a')
$arraylist.add(2)
$arraylist.add('iii')

$arraylist
```

When running the above you'll notice the output contains the indexes of each item we added to the [arraylist] before we printed the contents. If you are wanting to add 1000s of items, you'd have 1000s of lines of output.

```powershell
$arraylist = New-Object System.Collections.ArrayList

$arraylist.add('a')   | Out-Null
$arraylist.add(2)     | Out-Null
$arraylist.add('iii') | Out-Null

$arraylist
```

By adding `| Out-Null` we no longer get any of the results of adding items to the ArrayList, we've supressed the output.

There is another way to do this that avoids the use of a pipeline which I reccomend over the above approach:
```powershell
$arraylist = New-Object System.Collections.ArrayList

[void]$arraylist.add('a')  
[void]$arraylist.add(2)    
[void]$arraylist.add('iii')

$arraylist
```

[arraylist]: https://kasmichta.github.io/hjkl/docs/PowerShell/data-structures.html#arraylist
