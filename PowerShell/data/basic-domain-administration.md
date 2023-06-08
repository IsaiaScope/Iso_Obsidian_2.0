
What are Script Execution Policies?
  Policies that are designed to assist users
	Users assign basic rules
  Prevent users from unintentionally violating those rules
  They are NOT a security system
  They can be easily bypassed


```
Get-ExecutionPolicy -List

// Scope ExecutionPolicy
//	MachinePolicy Undefined
//	UserPolicy Undefined
//	Process Undefined
//	CurrentUser Undefined
//	LocalMachine Undefined

// undefined = not allowed
```


```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser 
``` 
consente gli script

Execution Policy Precedence
	1. Group Policy: Computer Configuration
	2. Group Policy: User Configuration
	3. Execution Policy: Process (or powershell.exe -ExecutionPolicy)
	4. Execution Policy: CurrentUser
	5. Execution Policy: LocalMachine
