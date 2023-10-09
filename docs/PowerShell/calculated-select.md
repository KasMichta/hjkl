---
layout: default
title: Calculated Select-Object Properties
parent: PowerShell
nav_order: 5
---
# Select-Object, but fancy

## Calculated properties

```powershell
Some-Cmdlet | Select-Object SomeProperty, @{Name="Calculated Property"; Expression={Calculation on $_ item}}
```

## Short Form

```powershell
Some-Cmdlet | Select-Object SomeProperty, @{l="Calculated Property";e={Calculation on $_ item}}
```

## Explanation

Really useful cmdlet for filtering data you get from any other cmdlet, Run the below:

```powershell
Get-CimInstance -ClassName Win32_LogicalDisk
```

That's too much info, let's *select* what we want:

```powershell
Get-CimInstance -ClassName Win32_LogicalDisk | Select-Object DeviceID, Size, FreeSpace
```

Well that's better, but those numbers aren't super nice to look at, let's convert them to gigabytes. There's a couple way to do this.

First, there are 1024^3 bytes in a gigabyte. So this would work, please refer to [PowerShell Maths] if you want to know where 'Pow' comes from:

```powershell
$Bytes = 254885666816

# The below divides bytes, by the amount of bytes in a gigabyte
'{0} GB' -f ($Bytes/[Math]::Pow(1024,3))
```

However, there's a much easier method built-into Bowershell:

```powershell
$Bytes = 254885666816

# I consider this cheating
'{0} GB' -f ($Bytes/1GB)
```

Much better, but why don't we get rid of all those unnecessary decimals:

```powershell
$Bytes = 254885666816

# I consider this cheating
'{0} GB' -f [Math]::Round($Bytes/1GB,2)
```

Great, but let's just get this right from the start with a calculated property, remember `$_` represents each entry from the cmdlet before the `|` being fed in:

```powershell
Get-CimInstance -ClassName Win32_LogicalDisk | Select-Object DeviceID, @{l="Size";e={'{0} GB' -f[Math]::Round($_.Size/1GB)}}, @{l="FreeSpace";e={'{0} GB' -f[Math]::Round($_.FreeSpace/1GB)}}
```

WOW! Amazing output, more amazingly, potentially one of the ugliest and horrendous to read lines of code you can find. Calculated properties are useful, but note, if it ends up looking like that, please stop and try the below which will make your code easier to read and easier to go back to in 4 years when it breaks.

## Using Variables to separate out calculated properties

```powershell
$prettySize = @{
	l="Size";
	e={
		'{0} GB' -f[Math]::Round($_.Size/1GB)
	}
}

$prettyFreeSpace = @{
	l="FreeSpace";
	e={
		'{0} GB' -f[Math]::Round($_.FreeSpace/1GB)
	}
}

Get-CimInstance -ClassName Win32_LogicalDisk | Select-Object DeviceID, $prettySize, $prettyFreeSpace
```
Now there's some maintainable lines of PowerShell. I think.

[PowerShell Maths]: https://kasmichta.github.io/hjkl/docs/PowerShell/maths.html
