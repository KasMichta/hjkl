---
layout: default
title: The Switch Statement
parent: PowerShell
nav_order: 92
---
# Like IF, but different
Please ensure you have a solid understanding of [IF Statements] before continuing with this article.

Let's say you have some condition to evaluate and depending on its value you might have a whole heap of different actions.
For example let's say your condition can output 1,2,3,4,5 and depending on each number you want to do something different. We could do this with `elseif {}` [statements].

This can get pretty long and difficult to work with if you have loads of different options, and becomes fairly painful to maintain.

For this sort of function we have the `switch`statement. 

```powershell
switch(Expression) {
	Result1 { Do-something}
	Result2 { Do-somethingElse}
}
```

It evaluates anything inside its `()` brackets, and then carries out some corresponding action. Here is an example of a switch statement:

```powershell
$value = 4

switch ($value) {
	2 { 'We have a 1'}
	4 { 'We have a 4'}
	6 { 'We have a 6'}
}
```

## More than 1 input
Switches are great for evaluating multiple input values, a switch will sequentially run the inputs through left to right.

```powershell

switch (5,1,3) {
	1 { 'itsa 1'}
	2 { 'itsa 2'}
	3 { 'itsa 3'}
	4 { 'itsa 4'}
	5 { 'itsa 5'}
}
```

## Break
Just like with [For Each Looping] we can stop the checks early.

```powershell

switch (5,1,3) {
	1 { 'itsa 1'}
	2 { 'itsa 2'}
	3 { 'itsa 3'}
	4 { 'itsa 4'}
	5 { 'itsa 5'; Break}
}
```

We only get the first result because we've instructed the switch statement to stop with `break`.

## Default
If we want something to happen when the value doesn't match anything, we can have a `default` block:

```powershell
switch ("this") {
	1 {"It is one"}
	2 {"It is two"}
	"three" {"It is three"}
	Default {
		"What is this?"
	}
}
```

## Wildcard
In the case you need a partial match you can use switch with a `-wildcard`:

```powershell
switch -wildcard ("119-281-299-919") {
	1 { "1" }
	2 { "2" } 
	"*299*" { "bingo"}
}
```

## Regex
Similarly Regex can be used instead:

```powershell
switch -regex ("1234-12-123-1234") {
	"^\w$" {"letter"}
	"^\d%" {"digit"}
	"^\d{3}-$" {"three digits followed by a -"}
	"^\d{4}-\d{2}-\d{3}-\d{4}" {"magic"}
	 
}
```

## Using Script Blocks as Conditions
```powershell
switch (4) {
	{$_ -is [String]} {
		"Found a String"
	}
	{$_ -is [Int32]} {
		"Found a Number"
	}
}
```

If the above is not making a ton of sense, refer to the below:
[$_ Variable]
[-is determinations]

[$_ Variable]: https://kasmichta.github.io/hjkl/docs/PowerShell/psitem.html
[-is determinations]:https://kasmichta.github.io/hjkl/docs/PowerShell/operators.html#-is-determinations
[IF Statements]: https://kasmichta.github.io/hjkl/docs/PowerShell/if-else.html
[statements]: https://kasmichta.github.io/hjkl/docs/PowerShell/if-else.html#or-else
[For Each Looping]: https://kasmichta.github.io/hjkl/docs/PowerShell/foreach.html

