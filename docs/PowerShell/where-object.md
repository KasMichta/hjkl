---
layout: default
title: Where-Object In-Depth
parent: PowerShell
nav_order: 6
---
# Where-Object
The powerhouse of the shell is the mi-**Where-Object**

## Simple usage

```powershell
Get-Service | Where-Object {$_.Name -like "*Update*"}
```

Beware, a lot of commands have built in features for filtering such as a `-filter` parameter. Where-Object is great for times where you are quickly getting information or performing ad-hoc work. When writing a script, refer to the documentation for a cmdlet to see if it already has a filtering method built-in, more often than not it will be lightyears quicker than where-object, this is especially important when handling 10,000s and 100,000s+ records.

For example, `Get-Service` has fantastic filtration achieved through `-DisplayName`, `-Include`, `-Exclude`. It's even implicit:

```powershell
Get-Service *Update*
```


## Explanation
Where-0bject can be treated as a filter. The cmdlet before where-object gives you `X` records. These records may all have properties such as `X.Name` or `X.Location`. Let's make a collection of these (Refer to [PSCustomObject] for quick reference, or [Data Structures] for in-depth).

```powershell
# ArrayLists are great, I'd highly recommend researching why
$List = New-Object -TypeName "System.Collections.ArrayList"


# Don't worry about the
# | Out-Null
# This just supresses a counter

$List.Add([PSCustomObject]@{
	Name     = 'Mario'
	Location = 'Italy'
}) | Out-Null

$List.Add([PSCustomObject]@{
	Name     = 'Luigi'
	Location = 'Italy'
}) | Out-Null


$List.Add([PSCustomObject]@{
	Name     = 'Tina'
	Location = 'Bulgaria'
}) | Out-Null

$List

```

## Remember Get-Member
So each entry in our `$List` has a name and a location. These are properties of the values, let's test. We'll choose the first `[0]` value of our `$List` and check it's properties using `Get-Member`. 

```powershell
$List = New-Object -TypeName "System.Collections.ArrayList"
$List.Add([PSCustomObject]@{ Name = 'Mario'; Location = 'Italy' }) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi'; Location = 'Italy' }) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Tina';  Location = 'Bulgaria' }) | Out-Null 


#This line is fine, but for speed when you're doing stuff, you can use the alias 'gm'
#$List[0] | Get-Member

$List[0] | gm
```

You can see that the item in the list has the `NoteProperty` for `Location` and `Name`.

# `$_`
So when we use Where-Object we are usually choosing a property and then filtering on the value in it. In the below, we're instructing PowerShell to go through the items in `$List` and only show an item if it's Location equals "`-eq`" Italy.
(`$_` means item, where-object is running 3 times because there's 3 items, `$_` is the placeholder)

For more information on `$_` refer to [$_ Variable]

```powershell
$List = New-Object -TypeName "System.Collections.ArrayList"
$List.Add([PSCustomObject]@{ Name = 'Mario'; Location = 'Italy' }) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi'; Location = 'Italy' }) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Tina';  Location = 'Bulgaria' }) | Out-Null 

$List | Where-Object {$_.Location -eq 'Italy'}
```

One of the records in the list did not have Italy in the "Location" property so it did not show.

Think of the following line:
```powershell
$List | Where-Object {$_.Location -eq 'Italy'}
```

As if it were doing this (Run it):
```powershell
$List = New-Object -TypeName "System.Collections.ArrayList"
$List.Add([PSCustomObject]@{ Name = 'Mario'; Location = 'Italy' }) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi'; Location = 'Italy' }) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Tina';  Location = 'Bulgaria' }) | Out-Null 

# Instead of $List | Where-Object {$_.Location -eq 'Italy'}
If ($List[0].Location -eq "Italy") {
	$List[0]
}
If ($List[1].Location -eq "Italy") {
	$List[1]
}
If ($List[2].Location -eq "Italy") {
	$list[2]
}
```

You'll notice the results are the same as the Where-Object version, because technically, it's the same operation. Where-Object allows us to abstract away the `$List[X].Location` , we don't need to know `X`, the list could have 1000s of values. Therefore `$_` is itself an incrementor, it will go through 0, 1, 2, 3, ..., `X` .

## Multiple queries
Let's make the list a bit more complex, add another property and some more records.

```powershell
$List = New-Object -TypeName "System.Collections.ArrayList"
$List.Add([PSCustomObject]@{ Name = 'Mario'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Tina';  Location = 'Bulgaria' ; Profession = 'Lawyer'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Tony';  Location = 'America' ; Profession = 'Pizza Chef'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Louis';  Location = 'United Kingdom' ; Profession = 'Plumber'}) | Out-Null 

$List
```

Now, let's filter again but this time let's use two queries. Be aware of the use of a **wildcard** after the `-like` operator. `*` means "anything" (letters, numbers, nothing, any amount of anything. So `"*a*"` would look for anything that has "a" anywhere in the value. Whereas `"*a"` would look for anything where there is an "a" at the end).

```powershell
$List = New-Object -TypeName "System.Collections.ArrayList"
$List.Add([PSCustomObject]@{ Name = 'Mario'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Tina';  Location = 'Bulgaria' ; Profession = 'Lawyer'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi';  Location = 'America' ; Profession = 'Pizza Chef'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Louis';  Location = 'United Kingdom' ; Profession = 'Plumber'}) | Out-Null 

# Where name starts with an l
$List | Where-Object {$_.Name -like "l*"}

[Environment]::NewLine

# Where the profession is Plumber
$List | Where-Object {$_.Profession -eq "Plumber"}
```

You'll see two separate lists, however, the property headings only appear once, this is just because PowerShell is trying to be smart with us. Ignore that.

Let's say we wanted the items where the Name started with an "l" AND the profession was plumber.

```powershell
$List = New-Object -TypeName "System.Collections.ArrayList"
$List.Add([PSCustomObject]@{ Name = 'Mario'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Tina';  Location = 'Bulgaria' ; Profession = 'Lawyer'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi';  Location = 'America' ; Profession = 'Pizza Chef'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Louis';  Location = 'United Kingdom' ; Profession = 'Plumber'}) | Out-Null 

$List | Where-Object {
	($_.Name -like "l*") -and 
	($_.Profession -eq "Plumber")
	}

```

Perfect, the expressions are now in `()` brackets, and there's a `-and` operator between them that provides the logic ([All the operators you can use](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logical_operators?view=powershell-7.2)).
So we can try now to get values based on either condition. Get us items where name starts with "l", or they are a plumber:

```powershell
$List = New-Object -TypeName "System.Collections.ArrayList"
$List.Add([PSCustomObject]@{ Name = 'Mario'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Tina';  Location = 'Bulgaria' ; Profession = 'Lawyer'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi';  Location = 'America' ; Profession = 'Pizza Chef'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Louis';  Location = 'United Kingdom' ; Profession = 'Plumber'}) | Out-Null 

$List | Where-Object {
	($_.Name -like "l*") -or
	($_.Profession -eq "Plumber")
	}

```

## Regex with `-match`
Well, what if we only want the Plumbers who have only one "i" in their name...

...regex.

This is a whole other subject that takes some time to learn, I suggest [This](https://adamtheautomator.com/powershell-regex/) and [This website that lets you test regex and explains what the query means.](https://regex101.com/)

Let's assume we know regular expressions very well, so this is a nice and easy one: `'^[^i]*i[^i]*$'`

Of course I am kidding, but for the below you will need at least a base understanding of regex operators.

Let's break it down:
`^` = line/word begins
`[^i]*`  = a character that is NOT "i", and appears 0 or more times
`i` = the letter "i"
`[^i]*` = a character that is NOT "i", and appears 0 or more times
`$` = end of line/word

So in human terms, only match when the word has an "i", but no letters before or after are "i"s. Note this would also work if the word started with an "i", like "Iago", or ended in an "i" like "Anatoli". This is because the non "i" letters (found with the negated set `[^i]`) are allowed to appear **0** or more times.

```powershell
$List = New-Object -TypeName "System.Collections.ArrayList"
$List.Add([PSCustomObject]@{ Name = 'Mario'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Tina';  Location = 'Bulgaria' ; Profession = 'Lawyer'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi';  Location = 'America' ; Profession = 'Pizza Chef'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Louis';  Location = 'United Kingdom' ; Profession = 'Plumber'}) | Out-Null 

$List | Where-Object {$_.Name -match '^[^i]*i[^i]*$'}

```

Regular Expressions are extremely powerful, so it's worth learning the basics at the least. If you have a complex query, a regex can handle it in one go, whereas you would need several `-like` conditions otherwise. Careful though, you need to make sure you know exactly what you regex is doing before using it in a bulk script, it can be easy to accidently over-match if your query is not specific enough.

## [](#where).Where()
The `.Where()`  method is also a shorthand way of performing filtering.

```powershell
$List = New-Object -TypeName "System.Collections.ArrayList"
$List.Add([PSCustomObject]@{ Name = 'Mario'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi'; Location = 'Italy' ; Profession = 'Plumber'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Tina';  Location = 'Bulgaria' ; Profession = 'Lawyer'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Luigi';  Location = 'America' ; Profession = 'Pizza Chef'}) | Out-Null 
$List.Add([PSCustomObject]@{ Name = 'Louis';  Location = 'United Kingdom' ; Profession = 'Plumber'}) | Out-Null 

$List.where({$_.Name -like "*u*"})
```

This is where things get a little strange because you can put `.Where()` methods inside Where-Object queries, however, I won't show you how to do that. Because I don't want you to suffer.

## Using Where-Object to marry lists
This is one of the secret uses of Where-Object, you can use it to give values from one set of records that match/fit a query based on a second set of records.

Let's say you have a list of companies.
You also have a list of services, and these services have a property of the company they are linked to.

Now let's say you want the companies that are not receiving any services (run the below):

```powershell
$companies = @('Company a', 'Company b', 'Company c', 'Company d')

# we have 'service a' and 'service b' offered to all but 'Company b'
$services = @(
	@{service = 'serv a'; company = 'Company a'},
	@{service = 'serv a'; company = 'Company c'},
	@{service = 'serv a'; company = 'Company d'},
	@{service = 'serv b'; company = 'Company a'},
	@{service = 'serv b'; company = 'Company c'},
	@{service = 'serv b'; company = 'Company d'}
	
)

$companies | Where-Object {$services.company -notcontains $_}
```

As you can see, we are asking for items from `$companies`, but only if they are not listed under any `.company` property in `$services`.

An even simpler example (this time using Where-Object alias `?`):

```powershell

$listA = @('apple', 'banana', 'carrot', 'date')
$listB = @('apple', 'banana', 'carrot')

$listA | ? {$listB -notcontains $_}
```

## The shorthand `?`
For the quickest use of Where-Object, we can avoid using words entirely. Here's the very first command from this document with the shortest Where-Object declaration:

```powershell
Get-Service | ? {$_.Name -like "*Update*"}
```

[PSCustomObject]: https://kasmichta.github.io/hjkl/docs/PowerShell/pscustomobject.html
[Data Structures]: https://kasmichta.github.io/hjkl/docs/PowerShell/data-structures.html
[$_ Variable]: https://kasmichta.github.io/hjkl/docs/PowerShell/psitem.html

