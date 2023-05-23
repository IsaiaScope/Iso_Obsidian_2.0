- `$` used for declaring a variabile
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