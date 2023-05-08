How to Use PSDrive?
The PSDrive cmdlet allows you to view, create and remove PowerShell drives. A PSDrive is considered a data store location that can represent the file system, a registry hive and network share, among other things as well.
• Go ahead and open PowerShell as the current user. We will use the cmdlet New-PSDrive to create a temporary or persistent drive that is mapped to or associated with a location in a data store.
To see the syntax of PSDrive type
```
 Get-Help New-PSdrive
```
• We will be using the command New-PS Drive and the parameters Name, PSProvider and Root.
• In our example, daily on a machine, you need to check a particular registry key. You have to open the registry editor, and drill down to that specific registry hive. In this case HKLM, and find the registry key.
I'm going to show you how to use New-PS drive to map that registry location to a name. And make it assessable like any file system drive.
• Now go ahead and copy and paste the first command into PowerShell.
**Command #1**
```
New-PSDrive -Name PSReg -PSProvider Registry -Root HKLM:\SOFTWARE\Microsoft\PowerShell\3\
```
Now what are the following parameters used for? 
- Name specifies a name for the new drive 
- PSProvider this shows that the drive is associated with the registry. 
- Root specifies the data store location to which a PowerShell drive is mapped. And HKLM is the target registry hive
To access the newly created PSDrive Type 
```
cd psreg:
```
don't forget the colon just after the psreg: name, otherwise you will get an error.
Now type 
```
dir
```
And there is our registry entries.

• Additional PSDrive cmdlets: 
- Get-PSDrive. This command that gets the drive in the current session. 
- New-PSDrive creates a PowerShell drive that's mapped to a location in a data store, such as a network drive, a directory on the local computer, or a registry key.
- Remove-PSDrive. This cmdlet deletes our PowerShell drives that were created by using the New-PSDrive cmdlet.

---

Now we'll create a PSDrive and map it to a local folder. Now will copy and paste the second command into PowerShell.

**Command #2**
```
New-PSDrive -Name "Win" -PSProvider "FileSystem" -Root “C:\windows\media”
```
Now for parameters, -Name specifies the name for the new drive. In this case it will be win. The -PSprovider parameter, it'll be file system and the -root will be the folder C:\windows\media.
To access your new PSDrive, type 
```
cd win:
``` 
then type 
```
dir
```
•` One important point to note here is that all your PSdrives are non-persistent and that means they'll disappear as soon as your session is closed.`
To make your PSDrive persistent and last even if your session is closed, you can map the PSDrive to appear in Windows Explorer using the switch -persist.
• Now copy and paste the third command into PowerShell.
**Command #3** 
```
New-PSDrive -Name 'L' -PSProvider FileSystem -Root '\\127.0.0.1\C$\Recycler -persist
```
Here are the parameters we will be using -name specifies the name for the new drive. In this case the name is L. -PSProvider is associated with the file system for the -Root we're using 127.0.0.1 and what this means is the root parameter can be associated with a network share. But in this case, we're using the loop back IP address for the local machine. C$ represents the local C:\recycle folder. And as we already said, -persist maps the drive to Windows Explorer.

To access the drive type cd space L: and type dir and press return. And there's our files.
Now close the PowerShell session and open Windows Explorer. You should see L: as your new persistent PowerShell drive with all the files and folders.
• Here's a tip if you recall. I opened PowerShell as the current user and not as the administrator. In this case, your mapped drives will only appear in Windows Explorer when you're logged into Windows as the current user.
• Now let's get back into PowerShell. To remove our persistent PSDrive, go ahead and type 
```
Remove-PSDrive -name L
```
Now, if we open windows explorer. We see that our L drive has been removed.
