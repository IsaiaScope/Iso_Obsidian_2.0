- What is a PowerShell Function?
Blocks of code with a specified identifier
You can execute the blocks of code inside of the function by calling its identifier
Allow you to avoid redundant blocks of code
- Function Scopes
A scope defines how functions are accessed
By default, if a function is a part of a script, it is available inside the script
By default, variables inside of a function are not available outside the
function
- Available Function `Scopes`
  You can declare the PowerShell Function scopes below (usually not necessary)
	- Global
    This scope is in effect when PowerShell Starts
	- Local
    Whatever current scope you are using for your script (could be any in this list)
	- Script
    This scope is created while the script runs
	- Private
    Items in this scope cannot be seen outside of this scope
	- [Number Scopes]
    Use a number (0, 1, 2 etc)
    These numbers represent a scope level above the current scope
    0 = Current Scope
    1 1 = One scope level above
    2 = Two scope levels above
- Order is important call function after declaration
---
- ex_1
```
Function Add {
     echo ($args[0] + $args[1])
 }
Add 5 10                
```
- ex_2 => param order
```
Function EchoText {
    param (
        $FirstParameter,
        $SecondParameter
    )
                     
    echo ("First: " +  $FirstParameter)
    echo ("Second: " + $SecondParameter)
}

EchoText -SecondParameter "This is the second" -FirstParameter "This is the first"         
```
```
Function EchoText ( $FirstParameter, $SecondParameter) {
    echo ("First: " +  $FirstParameter)
    echo ("Second: " + $SecondParameter)
}

EchoText -SecondParameter "Hey" -FirstParameter "Cara"             
```