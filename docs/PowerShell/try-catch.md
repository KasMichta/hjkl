---
layout: default
title: Try Catch Error Identification
parent: PowerShell
nav_order: 91
---
# Retrieving Errors
This gets last error and gives full name that can be used in a `Try{}Catch [x] {}` statement.

```powershell
$Error[0].Exception.GetType().FullName
```

## Explanation
For writing scripts that may have errors it's best to use Try{} Catch{} statements

```
Try{
	Do something here, this might fail
} Catch {
	If it does fail, do this
}
```

In most cases this can be pretty simple:

```powershell
Try{
	Get-Item C:\DoesNotExist\ -ErrorAction Stop
} Catch {
	Write-Output "That command failed."
}
```

The important part is having `-ErrorAction Stop` at the end of the command/s that might fail, this ensures that the Catch statement is ran.

However, you can actually drill down if multiple things can go wrong based on the error. Run the below:

```powershell
Get-Item C:\DoesNotExist
```

This is not a generic error, it's specific to the type of problem that PowerShell has recognised. Run the below to retrieve the actual type of the error that we can use in our Catch statement. **The Error name will appear first simply due to how we are running this in obsidian**

```powershell
Get-Item C:\DoesNotExist

$Error[0].Exception.GetType().FullName
```

You'll see it's a `System.Management.Automation.ItemNotFoundException`
We can then use this with the catch statement.

```powershell
Try{
	Get-Item C:\DoesNotExist\ -ErrorAction Stop
} Catch [System.Management.Automation.ItemNotFoundException]{
	Write-Output "Folder not found."
} Catch {
	Write-Output "Something else went wrong."
}
```

Now we have error-handled the folder not existing. There's a second catch statement in the above that will capture any other error, like for example...not writing the command correctly.

```powershell
Try{
	Get-Itarstawftem C:\DoesNotExist\ -ErrorAction Stop
} Catch [System.Management.Automation.ItemNotFoundException]{
	Write-Output "Folder not found."
} Catch {
	Write-Output "Something else went wrong."
}
```

If you are aware of multiple different errors that can occur you can use sequential `Catch [Error1] {} Catch [Error2]` and so on to Error handle them and either fix them, or present easy to read error messages.

```powershell
Try{
	Get-Itarstawftem C:\DoesNotExist\ -ErrorAction Stop
} Catch [System.Management.Automation.ItemNotFoundException]{
	Write-Output "Folder not found."
} Catch [System.Management.Automation.CommandNotFoundException]{
	Write-Output "Please check spelling of the command in the Try statement"
} Catch {
	Write-Output "Ooops, something unexpected went wrong."
}
```

## Using Try, Catch to learn about the error

When a Try/Catch statement catches an error it passes the value into the [$_ Variable], which can be accessed within the `Catch` block to output information about the error:

```powershell
Try{
	Get-Itarstawftem C:\DoesNotExist\ -ErrorAction Stop
} Catch {
	$_.Exception
	Write-Host "This error exception is of type:" $_.Exception.GetType()
}
```
[$_ Variable]: https://kasmichta.github.io/hjkl/docs/PowerShell/psitem.html
