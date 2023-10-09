---
layout: default
title: Comparison and Logical Operators
parent: PowerShell
nav_order: 94
---
## Comparison and Logical Operators 

The operators that PowerShell uses are as follows. For a full breakdown of ALL types of operators in PowerShell go here: [Link](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7.2) there's lots of wisdom on this page that I could have used years ago, great if you know a bit of PowerShell already.

### Comparative Operators

| Operator       | Function                                          |
| -------------- | ------------------------------------------------- |
| `-eq`          | Equals                                            |
| `-ne`          | Not Equals                                        |
| `-gt`          | Greater Than                                      |
| `-lt`          | Less Than                                         |
| `-le`          | Less or Equal to                                  |
| `-ge`          | Greater or Equal to                               |
| `-match`       | Regex, it's not a method the jedi would teach you |
| `-notmatch`    | Negated version of `-match`                       |
| `-like`        | Pattern Matches (can use `*` ) wildcards          |
| `-notlike`     | Negated version of `-like`                        |
| `-in`          | Value on left is one of `(x, y, z)` values        |
| `-notin`       | Negated version of `-in`                          |
| `-contains`     | Value on left contains value on right             |
| `-notcontains` | Negated version of `contains`                                                  |

These are what will let you generate `$true` or `$false` statements:

```powershell

'Statement 1'
45 -eq 46

'Statement 2'
'sink' -ne 'toilet'

'Statement 3'
20 -lt 100

'Statement 4'
30 -ge 30.0

'Statement 5'
"1234-1233-123-09090" -match "\d{4}-.*-...-09090"

'Statement 6'
"blue man group" -like "blue ma*"

'Statement 7'
7 -in (3,2,3,2)

'Statement 8'
@("India", "Korea", "Singapore") -contains "Korea"
```

### Logical Operators

Sometimes we want to string together some statements to create an output instead of just one condition for this we need logical operators.

| Operator      | Function                                    |
| ------------- | ------------------------------------------- |
| `-and`        | `$true` when both statements are true       |
| `-or`         | `$true` when either statement is true       |
| `-xor`        | `$true` when **only one** statement is true |
| `-not` or `!` | makes `$true` into `$false`, and vice versa | 

We only need to put the conditions that these are operating on inside `()`.
Examples:

```powershell
'Statement 1'
(1+1 -eq 2) -and ('l' -lt 'llllllll')

'Statements 2'
('height' -like 'hei*') -or (2 -lt 1)
($true) -or ($true)

'Statements 3'
($true) -xor ($false)
($true) -xor ($true)

'Statements 4'
-not $true
!$false
```

## [](#is)`-is` determinations

If looking to evaluate whether a value is of a certain type we can use `-is` and `-isnot`.

```powershell
$date = Get-Date

$date -is [DateTime]

4 -isnot [System.Int32]

4 -isnot [System.String]

"word" -is [System.String]
```

For more information on Data Types [Data Structures].

[Data Structures]: https://kasmichta.github.io/hjkl/docs/PowerShell/data-structures.html
