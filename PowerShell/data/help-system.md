- DOC: [get-help](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-help?view=powershell-7.3)
- **Get-Help** => Displays information about PowerShell commands and concepts.
- syntax => `get-help <cmd> <flags>`
- Examining the **HELP SYSTEM**:
	- examples
	- detailed
	- full
	- online

1. Get 11 examples of Get-Service cmd
```
Get-Help Get-Service -Examples
```
2. Get 11 examples of Get-Service cmd with more info:
	- syntax
	- description
```
get-help Get-Service -Detailed
```
3. Get 11 examples of Get-Service with all available information:
```
get-help Get-Service -full
```
4. open wiki on Get-Service cmd
```
get-help Get-Service -online
```
5. get all information in a list form that contain word 'process'
```
Get-Help *process*
```
6. open the result on a external window
```
Get-Help Get-EventLog -ShowWindow
```