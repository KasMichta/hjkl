---
layout: default
title: $_ Variable
parent: PowerShell
nav_order: 97
---
# The Pipeline Variable `$_`

The `$_` variable is simply defines as the "current value in the pipeline".
It is an alias for the `$PSItem` variable. If you are not yet familiar with the PowerShell Pipeline, please set aside about 30mins and go through this article: [The Pipeline]

```powershell
'x', 'y', 'z' | % { 'current value in the pipeline {0}' -f $_}
```

# Official name - `$PSItem`

Same function:

```powershell
'x', 'y', 'z' | % { 'current value in the pipeline {0}' -f $PSItem}
```

[The Pipeline]: https://kasmichta.github.io/hjkl/docs/PowerShell/pipeline.html
