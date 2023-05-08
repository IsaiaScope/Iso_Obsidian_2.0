So what are Providers?
Providers are a method to access data so that you can view this data in a hierarchical structure. 

For example, if you open Windows explorer, you view this data as a file system located in drives, directories and subdirectories. So with providers, they are designed to take various kinds of data storage and make it look like a disk drive. Providers have cmdlets that the user can run to manage these data stores. 

open Powershell ISE and click run-as Administrator.
```
Get-PsProvider
```

The list not only shows the built-in providers, but the drives that each provider currently supports. A drive is an entity that a provider uses to represent a data store through which data is made available to the PowerShell session. For example, the Registry provider creates a PowerShell drive for the HKEY_Current_User registry hive.
Now let’s take a look at all our drives. 
```
get-psdrive
```
This cmdlet is used to view all the PowerShell drives

Updating your help system
By now you are familiar with the PowerShell Help System. In this section We’ll be using it to display syntax and to get the latest examples for all the provider related cmdlets.
To update the Powershell help system we have to be running as an administrator.
```
Update-Help
```
• If you can update successfully great, If you get an error that displays, Failed to update Help for the module(s) PSReadline. This is a Microsoft issue.
Try running this command instead.
```
 Update-Help -Verbose -Force -ErrorAction SilentlyContinue
```
---
