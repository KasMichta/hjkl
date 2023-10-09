---
layout: default
title: PowerShell Maths
parent: PowerShell
nav_order: 98
---
# Math methods

```powershell
[System.Math] | Get-Member -Static -MemberType All
[Math] |Get-Member -Static -MemberType All
```

# Examples

### Pi

```powershell
[Math]::pi
```

### E

```powershell
[Math]::e
```

### Round

```powershell
[Math]::Round(15.3421111, 2)
```

### Square root

```powershell
[Math]::Sqrt(64)
```

### Power

```powershell
[Math]::Pow(2,5)
```

### Absolute

```powershell
[Math]::Abs(-5)
```

## Trig

### Sine

```powershell
[Math]::Sin(90)
```

### Cosine

```powershell
[Math]::Cos(90)
```

### Tangent

```powershell
[Math]::Tan(30)
```

## Log

```powershell
[Math]::Log10(1000)
```

## DATA units

```powershell
Write-Host "Values in Bytes:"
1KB
1MB
1GB
1TB
1PB
```

