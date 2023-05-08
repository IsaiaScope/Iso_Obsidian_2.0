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

`Location Cmdlets` – These cmdlets are used for directory navigation. 
The cd (change directory) command can be used to navigate between directories. But, as the number of directories that we need to track grows, this approach becomes more and more inefficient, as most of these paths are usually too long to type. And that’s why location cmdlets can be extremely useful.
`Get-Location` – (alias GL) This cmdlet gets an object that represents the current directory. Sets the working location to a specified location
`Set-Location` – (alias is SL) Sets the working location to a specified location. That location could be a directory, a subdirectory, a registry location, or any provider path.
`Push-Location` – (alias is pushd) Adds ("pushes") the current location onto a location stack.
`Pop-Location` – (alias is popd) Changes the current location to the location most recently pushed onto the stack by using the Push-Location cmdlet.

`What is a stack?` Think of a stack like a stack of books.
I add books by adding them to the top of the stack. Computer terms this is referred to “Last in, first out” or (LIFO)
I can demonstrate this by creating 4 folders.
Let’s say that we need to work with these four folders.
From your C: drive create a folder called books, then create the four sub-folders.

Now lets add all four folders to the stack by using our pushd alias cmdlet
```
pushd c:\books\1984
pushd c:\books\gulivers_travels
pushd c:\books\hamlet
pushd c:\books\moby_dick
```
Now lets add all four folders to the stack by using our pushd alias cmdlet
Now let’s view the stack by using the 
```
get-location -stack
```
(parameter) remember (LIFO) last in first out
Because get-location displays our current location, moby_dick is in the stack but not shown in the stack.

Now from PS C:\books\moby_dick location type popd
Type get-location -stack (moby_dick) has been removed from the stack)
From PS C:\books\Hamlet location type popd
Type get-location -stack now hamlet has been removed from the stack
From PS C:\books\Gulivers_Travels location type popd
Type get-location -stack (gulivers travels has been removed from the stack)
From PS C:\books\1984 location type popd
Type get-location -stack (1984 has been removed from the stack)
Type get-location -stack and now the stack is empty.

As you can see we can move between these four folders and easily remove the folders from the stack that are no longer needed.

`Set-Location`
You can use this cmdlet to access PowerShell drives. This cmdlet changes the current location to a new location. The location could be a registry location, folder or subfolder. To understand how this works type:
```
Set-Location c:\windows
Set-Location –Path Env:
Set-Location -Path HKCU:
```
**Command #1** 
```
set-location software\microsoft\windows\currentversion\uninstall
```
To drill down into the uninstall location you can use the Get-childitem cmdlet.
```
get-childitem
```
Get-childItem - Gets the items in one or more specified locations.
If you are installing software and you need the software version or the uninstall string, you can use this command to find that string.
Additional examples for using get-childitem Use get-childitem to Search for all the DOCX files in the Documents and settings folder and subfolders.

**Command #2** 
```
get-childitem -path ‘C:\Documents and Settings’ -include "*docx" -Recurse -file
```
Parameters:
-Path This parameter specifies the path of one or more locations.
-include This is a string parameter and when this parameter is used, it displays specific files and folders.
-recurse This parameter instructs powerShell commands such as Get-ChildItem to repeat in sub directories.
-file The file attribute gives an output of only files under that container

The errors are from the search not finding the correct path to the files.

--- 

Now let’s take a look at one of the `Item cmdlets`
New-Item - Creates a new item and sets its value. This can be a folder, file or multiple files inside a directory. Use 
```
get-help New-item
```
to view all the parameters.
let’s use the New-Item cmdlet to Create a folder on your C: drive called PSTest

**Command #3**
```
New-Item -Path C:\ -Name “PSTest” -Itemtype “directory”
```
For Parameters
-Itemtype Can be a file or a folder.
We can check for the folder in windows explorer.
Let’s Create a file called test1.txt inside the PStest directory, and add txt to the file.

**Command #4**
```
New-Item -Path C:\PSTest -Name “test1.txt” -Itemtype “file” -Value “Hey boss, I want a 50% raise.”
```
Parameters
-Value allows you to add txt as a value. And press return
And if you open windows explorer again, you can click on test1.txt and checkout our value Hey boss I want a 50% raise.

Now let’s try to overwrite an existing file using the -confirm parameter.

**Command: #5**
```
New-Item -Path C:\PSTest -Name “test1.txt” -Itemtype “file” -Confirm
```
• The result is that the -confirm parameter displays a warning, and asks you to confirm this.
• If you want to overwrite the existing file just add the -force parameter.

Let’s go ahead and create multiple files in a designated folder.

**Command #6**
```
New-Item -ItemType "file" -Path "C:\PSTest\child1", "C:\PSTEST\child2"
```
Now from Windows Explorer if we open the PSTest folder there are our two files that we just created.

`Content Cmdlets`
Now let’s take a look at Content cmdlets. These cmdlets Append, clear, get and replace the content of a file.
Add-Content – Appends content to a file.
• From the PSTest folder on your C: drive create a file called test.txt, use the command New-Item to create the file.
**Command #7** 
```
New-Item -path "C:\PSTest" -name "test.txt" -itemtype "file"
```
Use the add-content cmdlet to append the following text to the file. Let’s go to lunch by Noon.

**Command #8** 
```
Add-content "C:\PSTest\test.txt" -Value "Let's go to lunch by noon"
```
And open windows explorer and click on test.txt, and you will see the added txt that we appended to the file.
Now delete the contents of the test.txt file using the clear-content cmdlet.

**Command #9**
```
Clear-content "C:\PSTest\test.txt"
```
Now from Windows Explorer, If we take a look at test.txt we can see that the contents have been cleared.

---

In this lecture we will take a look at Item property and path cmdlets.
ItemProperty cmdlets
• New-ItemProperty - creates a new property for a specified item and sets its value. Typically, this cmdlet is used to create new registry values.
• Get-itemProperty gets the property of an item like viewing registry properties and their values. You can use this cmdlet to get information about directories, files or registry entries.
• Remove-ItemProperty – This deletes a property and its value from an item. You can use it to delete registry values and the data that they store.
Please note: That the registry entry must exist before you can edit the entry.
First, we need to create a new registry key in the HKLM registry hive. We will use the New-item cmdlet. In the file system, New-Item creates files and folders. In the registry, New-Item creates registry keys and entries.
Be sure to open PowerShell as an Administrator.
Using this command we will create a registry key named MyCompany.
Command #1
New-Item -Path "HKLM:\Software\MyCompany"
Press return
Displayed is our registry key MyCompany
Now we need to use the command New-itemproperty to add a new registry entry to the MyCompany registry key.
Command #2
New-Itemproperty -Path "HKLM:\Software\MyCompany" -name NewProperty -value NewPropertyValue
Press return
We just added the name NewProperty to the registry.

Now we need to add a new value of 400 employees to my company.
Command #3
New-itemproperty -path HKLM:\Software\MyCompany -name NoOfEmployees -value 400
Press return
And there is our employee values of 400.
Let’s take a look at the results by taking a look at the windows registry editor.
• From the search bar type registry
• From HKLM, follow the path software\MyCompany
• Click MyCompany
And there is our registry key and our entries.
Now let’s take a look at the results by using the get-itemproperty cmdlet.
Type CLS – Press return
Command #4
Get-itemproperty hklm:\software\mycompany
Press return
And you can see all our entries.
Now we will remove the registry value NewProperty that we created using Command #2
Command#5
Remove-Itemproperty -Path "HKLM:\Software\MyCompany" -name NewProperty
Press return
• Now run Command #4 again and notice that the NewProperty value has been removed.
Now we will remove the MyCompany registry entry by using the cmdlet remove-item

Command #6
Remove-Item -Path "HKLM:\Software\MyCompany"
Press return
Rerun Command #4 and you will see that MyCompany has been removed.
Path cmdlets
Now let’s checkout several of the path cmdlets
• Test-Path - Determines whether all elements of the path exist. This cmdlet is handy if you are writing a script and you want to test whether the path to a file is true or false. Test path can save you a lot of time.
For Example: Perhaps you need to check to see if the password log is present in the debug directory.
Command #7
Test-Path 'C:\windows\debug\PASSWD.LOG'
Press return
The cmdlet returns true, so we know that passwd.log file is in that folder.
Now let’s check out the Resolve-path cmdlet
• Resolve-Path - Displays the items and containers that match the wildcard pattern at the location specified.
Now let’s say you need to take a look at all the paths in the Windows directory.
Command #8
Resolve-path "C:\windows\*"
Notice the wildcard in the command.
Press return
This command lists all the folders, subfolders and files in the Windows directory.

• Let’s take a look at the Split-path cmdlet. Split-path returns a string that describes the location of the items and returns the specified part of a path. Let's say you want to list all the log files in this C:\Windows folder. You can use the split-path cmdlet.
Command #9
Split-Path -Path "C:\windows\*.Log" -Leaf -resolve
Press return
Now for Parameters we used:
-Path is all the log files located in C:\windows
-Leaf This command displays the files that are referenced by the split path. Because this path is split to the last item, also known as the leaf, the command displays only the file names.
-resolve The resolve parameter tells Split path to display the items that display pass references. Instead of displaying the split path.
At this point we've completed this lecture. Thanks for watching and we'll see you in the next lecture.