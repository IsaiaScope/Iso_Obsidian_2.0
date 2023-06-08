**Network Stack Info Retriever**
All of these commands are very useful for troubleshooting network issues, diagnosing network performance problems, and understanding the current state of the network on a Windows system.
The Command Line:
- Get the configuration of all network adapters
```
ipconfig /all
```
- Display the content of the DNS client resolver cache
```
ipconfig /displaydns
```
- Get network routes
```
route print
```
- Show the active TCP connections and listening TCP and UDP ports, with PIDs
```
netstat -ano
```
Here is the Explanation: You are probably familiar with most of these commands.
1. ipconfig /all displays detailed configuration information for all network adapters installed on the system, including the IP address, subnet mask, and default gateway for each adapter.
2. ipconfig /displaydns displays the contents of the DNS client resolver cache, which contains a list of recently resolved DNS names and their corresponding IP addresses.
3. route print displays the current network routing table, which shows how network traffic is directed from the local system to remote networks and hosts.
4. netstat -ano displays a list of all active TCP connections and listening TCP and UDP ports on the local system, along with the process ID (PID) of the program that opened each connection or port. This information can be useful for troubleshooting network connectivity issues or identifying network-related security threats.
Each of these commands provides different types of network-related information and can be used for different purposes. For example, ipconfig /all can be used to troubleshoot network connectivity issues, ipconfig /displaydns can be used to view the results of DNS name resolution requests, route print can be used to diagnose network routing problems and netstat -ano can be used to identify network-related security threats.
---
**Application Retriever**
You will need to open the student guides for each lecture so that you can copy and paste the scripts into Powershell
This one-liner searches the Windows registry and gets all the installed applications on a Windows 64-bit system. Displays version, publisher, and the install date in a formatted table. You can output to a txt file by adding > output.txt.
Now I’ll show you in the registry editor how this one-liner finds all the application information. I’ll open regedit in Admin mode, then click HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall On the right side, you will see the following properties: "DisplayName", "DisplayVersion", "Publisher", and "InstallDate". These registry keys are where our script will pull all the application information.
Let’s go ahead and open Powershell in Admin mode. Now from the student guide go ahead and copy and paste the command line.
Command Line:
```
Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table –AutoSize
```
Here is the explanation:
1. This command line uses the "Get-ItemProperty" cmdlet to retrieve information from the registry key "HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall*".
2. The pipe symbol "|" is used to pass the output of the "Get-ItemProperty" cmdlet to the "Select-Object" cmdlet.
3. The "Select-Object" cmdlet is used to choose and display specific properties from the retrieved information. In this case, it selects the following properties: "DisplayName", "DisplayVersion", "Publisher", and "InstallDate".
4. Finally, the output is formatted using the "Format-Table" cmdlet with the "-AutoSize" parameter, which adjusts the column width automatically based on the content to make it easier to read.

Overall, this one-liner provides a quick and easy way to retrieve a list of installed programs on a Windows 64-bit operating system, along with their version numbers, publishers, and installation dates.
Now lets go ahead and output the one-liner to a Txt file.
I have a folder called C:\test => So I can just add our greater than symbol (`output` redirection operator)
```
Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Format-Table –AutoSize >C:\test\output.txt
``` 

we see that our application information is all there, could use some formatting, but that all looks good. Ok, thanks for watching and I’ll see you in the next lecture.

---
**Running Process Retriever**
These commands in this one-liner are all PowerShell commands that retrieve information about running processes on a Windows system: `First, what is a Running Process? A running process is a set of instructions currently being processed by the computer processor.` One way to view running processes is to open Task Manager then click the Processes tab.
I’ll go ahead and open Task Manager – Take a look at the processes and their usage – CPU, Memory, disk, network, and GPU. I’m sure you are very familiar with task manager.
Now let’s open Powershell in Admin mode

```
tasklist /v
query process *
Get-Process
```
Here is the Explanation:
1. `tasklist /v` is a command that displays a list of all running processes on the system, along with additional information about each process such as its process ID, memory usage, and status.
2. query process * is a command that queries the system for information about all running processes and can display it in the command prompt window.
3. Get-Process is a PowerShell cmdlet that retrieves information about running processes on the local computer, including the process name, ID, and amount of memory and CPU time used.
All of these commands can be used to view the list of running processes on a Windows system, but they differ in their syntax and the level of detail provided. tasklist /v and query process * will work using the command prompt or in PowerShell. While Get-Process is a PowerShell only cmdlet. Get-Process provides more detailed information about processes than the other two commands and can be used to perform additional operations on processes, such as stopping or restarting the process.


To start a process, we would use the Start-Process cmdlet followed by the path to the executable file.
Here's an example, let’s say you want to start and stop Microsoft Word
In my case, because I have the latest copy of Word, I would type
```
Start-Process "C:\Program Files\Microsoft Office\root\Office16\WINWORD.EXE"
```
Press return and Microsoft Word starts
Now if you wanted to stop word. First, you would need to run the get-process command and at the bottom, we would
need to get the ID number for Winword.exe,
Type Get-Process, then press return
The ID #, in this case, is 22200
Then you would type
```
 Stop-Process – Id Then type the ID number
```
Now press return and Microsoft Word Stops.

---
**Active Directory Module checker for Powershell**
This one-liner checks to see if the `ActiveDirectory` module is present on a system.
Let’s go ahead and open Powershell in Admin mode and Copy and Paste the one-liner from the Student guide.
Here is the Command Line:
```
if($(Get-Module -ListAvailable) -match "ActiveDirectory"){"Module Present"}else{"Module Not Present"}
```
Here is the Explanation:
1. Get-Module -ListAvailable is a cmdlet that retrieves information about the PowerShell modules that are currently available on the computer.
2. $() is a subexpression operator that encloses the Get-Module -ListAvailable command, causing PowerShell to evaluate the command first and return its output as a result.
3. -match is a comparison operator that tests whether a string matches a specified pattern. In this case, the pattern being searched for is the string "ActiveDirectory".
4. The result of the comparison is a boolean value: $true if the pattern is found, $false if it is not found 1:16.
5. The if statement checks whether the result of the comparison is $true or $false. If the result is $true, meaning that the "ActiveDirectory" module is present, the code block "Module Present" is executed. If the result is $false, meaning that the "ActiveDirectory" module is not present, the code block "Module Not Present" is executed.
Therefore, if($(Get-Module -ListAvailable) -match "ActiveDirectory"){"Module Present"}else{"Module Not Present"} checks whether the "ActiveDirectory" module is present among the available PowerShell modules and returns a message indicating whether the module is present or not.
If you want to install Active Directory feature for Powershell on your Windows 10 computer:
1. Click Start, Settings, Apps, Optional Features
2. Click Add a Feature button at the top, and then scroll down and check RSAT: Active Directory Domain Services and Lightweight Directory Services Tools.
3. Click the Install button and Windows 10 will then enable the feature.
I have already installed the module, so if I run the Powershell one-liner
it should respond with module present.
For more information on RSAT click the link below [link_1](https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps)
And check this out as well: [link_2](https://learn.microsoft.com/en-us/windows-server/remote/remote-server-administration-tools?source=recommendations)

---

**Scheduled Task Retriever**
This script retrieves a list of running scheduled tasks on the local computer.
First, if you want to follow along, if you have not done so stop the video and create a folder on your C: drive called test.
Let’s open Powershell ISE in Admin mode

Go ahead and copy and paste the one-liner from the student guide into the Script Pane
Here is the Command Line:
```
Get-ScheduledTask | Get-ScheduledTaskInfo | Select TaskName,TaskPath,LastRunTime, LastTaskResult,NextRunTime,NumberofMissedRuns | Sort-Object -Property TaskName
```
Here is the Explanation:
Get-ScheduledTask: This cmdlet is used to retrieve the list of scheduled tasks on the local computer. It generates a list of TaskScheduler objects representing the tasks that are scheduled to run on the system.
Get-ScheduledTaskInfo: This cmdlet is used to get detailed information about the scheduled tasks. It retrieves information such as the LastRunTime, LastTaskResult, NextRunTime, and NumberofMissedRuns for each scheduled task.
Select: This cmdlet selects specific properties from the objects returned by Get-ScheduledTaskInfo. In this case, the script selects the task name, task path, last run time, last task result, next run time, and number of missed runs.
Sort-Object -Property TaskName: This cmdlet sorts the output returned by Select in alphabetical order based on the TaskName property.
You can Save your script file by clicking on File, Save as, then in this case I will name the file Get-ScheduledTasks.ps1 and save this script to the C:\test folder
Let’s clear the screen and verify that our script is working. From File, click New From the console Pane type cls and press return
To run your script, from PowerShell, click File, Open, and navigate to the folder where your script file is located, in this case, it's C:\test. Then, click the file name of the script file and click Open.
Then click the Run Script tab or press F5.
This will execute your script and display the scheduled tasks on the screen.

`Overall, this PowerShell script retrieves the list of scheduled tasks on the local computer, gets detailed information about each scheduled task, selects specific properties to display, and sorts the output alphabetically by the TaskName property.`
You now have a PowerShell script that can get the scheduled tasks from a Windows 10 computer.
Now let's say instead of displaying all the scheduled tasks on the computer, you want to check on just one scheduled task called TaskInfo that you previously configured.
Go ahead and clear the screen again and copy and paste this command from the student handout
Here is the Command Line:
```
Get-ScheduledTask -Taskname loginfo | Get-ScheduledTaskInfo 
```
Taskname loginfo: This part of the command specifies the name of the task that you want to retrieve information about. In this case, the task is named "loginfo".
• And the script runs every day at 9:00 AM

So there you have it, now you know how to retrieve all or just one of the scheduled tasks on a local computer.
If you need additional information about scheduled tasks check out the link in the handout. [link_1](https://learn.microsoft.com/en-us/powershell/module/scheduledtasks/?view=windowsserver2022-ps)

---
**Security Check - Uninstalling Media Player**
By default, Windows Media Player is installed on Windows Server 2019 and possibly later version as well. Because this tool is not needed on production servers, it is an unnecessary function and therefore a potential security vulnerability. To uninstall media player, perform the following. I have two one-liners that you could use. In this lecture, we will discuss both.
Go ahead and copy and paste the first command from your student guide
Here is the Command Line:
```
dism /online /Disable-Feature /FeatureName:WindowsMediaPlayer /norestart
```
Here is the Explanation:
This is a command-line script that uses the Deployment Image Servicing and Management (DISM) tool to disable the Windows Media Player feature on the currently running Windows operating system.
dism: This is the name of the command-line tool used to manage Windows images.
/online: This parameter specifies that the DISM command should be run on the currently running Windows operating system, as opposed to an offline Windows image.
/Disable-Feature: This parameter specifies that the feature being targeted should be disabled.
/FeatureName:WindowsMediaPlayer: This parameter specifies the name of the feature that should be disabled, which is "WindowsMediaPlayer" in this case.
/norestart: This parameter specifies that the computer should not be restarted after the command has completed.
So, when you run this script, it will use the DISM tool to disable the Windows Media Player feature on the currently running Windows operating system, without requiring a system restart.
Here is the 2nd Command Line: I’ll go ahead and copy and paste this command from the student guide. 
```
Disable-WindowsOptionalFeature -Online -FeatureName "WindowsMediaPlayer"
```
Here is the Explanation:
1. Disable-WindowsOptionalFeature is a cmdlet that allows you to disable or remove an optional Windows feature.
2. -Online is a parameter of Disable-WindowsOptionalFeature that specifies that the operation should be performed on the currently running operating system, rather than on an offline image.
3. -FeatureName "WindowsMediaPlayer" is another parameter of Disable-WindowsOptionalFeature that specifies the name of the Windows feature to disable or remove. In this case, the feature being disabled is "Windows Media Player".
To summarize: -Disable-WindowsOptionalFeature -Online -FeatureName "WindowsMediaPlayer" This command is a PowerShell one-liner that disables the "Windows Media Player" feature on the currently running operating system.
This is not something that I want to do on my Windows 10 computer. But as we have already said Windows media player is considered a security issue when running on a production server, so it is best to disable or remove this feature.




