**Create a Scheduled Task Using PowerShell**
In this example, we'll create a scheduled task to run a PowerShell script every day at a specific time.
First, you will need to create a folder on your local hard drive. In this case, I have created a folder called C:\test. If you want to follow along stop the video and create the same folder on your C: drive.
This time we will open PowerShell ISE in Admin mode.
We will now create and save a script. From the Student, guide copy and paste this command line into the script pane in Powershell ISE.
Command Line:
```
Get-Date | Out-File c:\test\loginfo.txt -Append
Get-PSdrive -PSProvider Filesystem | Out-File C:\test\loginfo.txt -Append
```
Explanation: Get-date get’s time and the date then copies that out to logfileinfo.txt
Then it gets the drive Names, the Used space in GB, Free space in GB on the drive then copies that out to loginfo.txt as well.
Click the run script button or press F5 to run the script. Now check C:\test to be sure the loginfo.txt file has been created. Let’s take a look at the text file.

So, the question is why would you want to use this script? Let’s say you have a server, and you want to keep track of hard disk usage. You could do that with this script.

Now back to Powershell, we will want to save this script and name it loginfo.ps1 From file, click save as then we will save this file in the C:\test\loginfo.ps1
Let’s clean up the screen, from File, New. Then in the console pane type cls and press return

Now let’s create our scheduled Task Copy and paste the script from the student guide into the top panel or script panel of Powershell ISE
Command: 
```
$action = New-ScheduledTaskAction -Execute 'Powershell.exe' -Argument 'C:\test\loginfo.ps1'
$trigger = New-ScheduledtaskTrigger -Daily -At 9 am
Register-ScheduledTask -Action $action -Trigger $trigger -Taskname “loginfo daily” -Description "My Students are Awesome”
```
Explanation:
1. $action = New-ScheduledTaskAction -Execute 'Powershell.exe' -Argument 'C:\test\loginfo.ps1'
This line creates a new scheduled task action that specifies the program to execute and any command-line arguments. In this case, the program to execute is PowerShell.exe and the argument is the path to
the PowerShell script "C:\test\loginfo.ps1". This means that when the scheduled task runs, it will launch PowerShell and run the specified script.
2. $trigger = New-ScheduledtaskTrigger -Daily -At 9 am
This line creates a new scheduled task trigger that specifies when the task should run. In this case, the trigger is set to run the task every day (-Daily) at 9 am (-At 9 am).
3. Register-ScheduledTask -Action $action -Trigger $trigger -Taskname “loginfo daily” -Description "My Students are Awesome”
Finally, this line registers the new scheduled task with the Task Scheduler service. The -Action parameter specifies the action to run (which was created in the first line of code), the -Trigger parameter specifies the trigger to use (which was created in the second line of code), and the -Taskname and -Description parameters are used to give the task a name and description. In this case, the task is named "loginfo daily".
4. Now we need to run this script and register our Scheduled task named Loginfo Daily
5. So, in summary, this PowerShell script creates a scheduled task that runs a PowerShell script every day at 9 am. The PowerShell script that runs will be "C:\test\loginfo.ps1". The scheduled task is named "loginfo daily" and will be registered with the Task Scheduler service.

Now let’s check out the Task scheduler Now from the Windows search bar, type `Task Scheduler`, then click the Task Scheduler. From the left side, click the Task Scheduler Library, right click then click refresh. Scroll down and you should see the scheduled task that you listed in the script in this case it’s loginfo daily.
There is so much that you can do with powershell to automate scheduled tasks, this is just one example among many.
If you want to learn more about scheduled tasks and Powershell check out these links: [link_1](https://www.alitajran.com/scheduled-task-powershell/), [link_2](https://adamtheautomator.com/powershell-scheduled-task/), [link_3](https://woshub.com/how-to-create-scheduled-task-using-powershell/)