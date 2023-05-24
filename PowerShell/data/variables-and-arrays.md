- `$` used for declaring a **variabile**
- can be declared in .ps1 files with following syntax => $var = 1
- is possible to assign variables run time, just write:
```
$var = 1
```
in terminal
- create a variables that value is assign run time
```
$UserInput = Read-Host -Prompt "Please enter your name"
```
- check var value => echo $UserInput
```
echo $UserInput
```
- can store process result directly
```
$process = Get-process
```
- write variables content in a external file
```
$UserInput = Read-Host "Please Enter your first name" // Please Enter your first name: Joe
$UserInput // Joe
$UserInput > File.txt // create a file named File that contains $UserInput value
cat .\File.txt // cat read files contet in this case: Joe
```
- same result writing in ps1 file
```
$UserInput = Read-Host "Please enter something to save in your file"
Set-Content -Value $UserInput -Path File.txt
```
---
- `@` is used for **arrays**
- declare and adding elements
```
$MyArray = @("Elm1", "Elm2", "elm3")
$MyArray = @() // declare empty array
$MyArray [0] // Elm1
$MyArray [3] // nothing
$MyArray += 'Apples' // add an element
$MyArray += @("Elm4", "Elm5", "elm6") // add multiple element
```
- arrays method:
- **SORT**
```
$Alphabet = ("C", "D", "A", "E", "B")

$Alphabet | Sort -Descending // N.B. not change data

$Alphabet = $Alphabet I Sort // to change array value you need to reassign it
```
- **REMOVE**: Where is like filter 
- -`ne => not equal`
```
$MyArray
	Apple
	Peppers
	Olives
$MyArray | where {$_ -ne "Peppers"} // this cmd doesn't change data without equal sign
	Apple
	Olives
$MyArray
	Apple
	Peppers
	Olives
$MyArray = $MyArray | where {$_ -ne "Peppers"}
$MyArray
	Apple
	Olives
$MyArray = $MyArray | where {$_ -ne $MyArray[1]}
	Apple
```
- Delete an array
```
$MyArray = $null
```
- es_1 Variable Challenge
  1. Store the user's first and last name in two separate variables
  2. Create a new TXT file in the following location => C:VariableChallenge (create the directory before running script)
  3. Name the new TXT file in the following format => Todays_Date-First_Name-Last_Name.txt
```
# Read user input
$firstName = Read-Host -Prompt "Please enter your first name"
$lastName = Read-Host "Please enter your last name"

# Get todays date with propper formatting
$today = Get-Date -Format "ddMMyyyy-HHmms"
$longDate = Get-Date -Format "F"

# write this information to the file
Set-Content -Value "
Fist Name: $fistName
Last Name: $lastName
Date: $longDate
" -Path "C:\VariableChallenge\$today-$firstName-$lastName.txt


```



