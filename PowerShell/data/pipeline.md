how we can use the pipeline to complete everyday tasks. We’ll also take a look at something
new called parameter binding.

We’ll talk about:
• What-if – With what-if, you can test your command before you try it.
• Send output to a file using out-file. get-help *out-* Using out-file
• Finally, we’ll talk about how PowerShell sends commands through the pipeline.

**1)** Let’s say we want to delete some files, but because we’re using wildcards, we don’t want to
delete the wrong files. We can take the cautious approach and use the -whatif command.
Let’s open explorer, go to our C drive
I have created a folder named Company with a subfolder named HR. I have created several txt
files within each folder.
• The goal is to delete all the files in HR and Company without deleting any of the other
files or folders on my c drive.
First, I’ll type the command then explain it
```
Get-ChildItem C:\Company\*.txt -Recurse | Remove-Item -WhatIf
```
Get-childitem – Is like using dir command
I created two folders C:\Company and C:\Company\HR
*.txt – The files to look for
-Recurse – tells PowerShell to Search Subdirectories, here’s our pipe operator.
Remove-item – Equivalent to delete
-whatif – Test but won’t complete our command.
Here we see whatif performing a test operation on these targets.

Now let’s type the same command but this time we’ll use the -confirm parameter
```
Get-ChildItem C:\Company\*.txt -Recurse | Remove-Item -confirm
```
PowerShell will ask you to confirm the deletion of each text file.
Or we could go ahead and say Yes to all, and it will delete all the files. And we see that
all our text files are gone.


**2)** Now let’s check out the out-file command.
• We’re going to write the contents of our application log out to a file named app.txt
```
get-eventlog -logname application | out-file c:\app.txt
```
Now let’s go to windows explorer and lets open the file in notepad. And as you can see
there is the contents of the application log.

What command in PowerShell can we use to look at the contents of the application log file.
```
get-help *content*
```
see that there is a get-content command
```
 get-content -path c:\app.txt
```
And our application log is displayed on the screen.


**3) THEORY** Let’s go a little deeper into the pipeline.
• The pipeline is used for connecting commands together
• Using the pipeline, we can pass the output of one command to the input of another
command.

`So how does PowerShell know which commands will work together on the pipeline?`
This time we’ll use get-service.
```
get-service -name bits | Stop-service
```
• BITS is the Background intelligent Transfer service. It is used to transfer files between
computers.
• Get-service is command #1
• Stop-service is Command #2
So, what actually happens when I run this command.
Get-service (1st command) is producing an object and then that object will go across the
pipeline. Stop-service which is the 2nd command will connect to that object by mapping it to a
parameter.
`So how does PowerShell know what parameters to choose?`
To make this happen there only four of pieces of information that PowerShell needs.
1. Command #1 We’ll look at the TypeName: (using get-member)
2. Can Command #2 use the same TypeName as Command #1 (use get-help)
3. Does Command #2 accept pipeline input (use get-help)
4. Can Command #2 accept pipeline input (ByValue or ByProperty)

Open two PowerShell Windows
• For command #1 
```
get-service -name bits | gm
```
And we see that our TypeName is ServiceController
• For Command #2
```
get-help stop-service -full
```
Now let’s scroll up until we find a parameter that matches the TypeName.
• So, for command#2 we see that -InputObject matches the Typename from Command1
called ServiceController.
• The next thing is, Does InputObject accept pipeline input and we see that’s True.
• Then, can InputObject accept pipeline input byValue, and it does that as well

Now let’s run our command to verify that it will work
```
get-service bits | stop-service -passthru
```  
And we see that the service called Bits has stopped
`To get a clearer picture of parameter binding and how PowerShell accomplishes this let’s
review.`
Let’s take a look at command 1 which is get-service
1. You need to use get-member to get the Typename. Which in this case is
ServiceController. This is the name of the object that is being pushed across the
pipeline.
2. Now for command 2 which is stop-service, we open the help system and for the
parameter -InputObject, we see three matches.
The Typename, which is service Controller
We see that it accepts pipeline input which is true
and it accepts pipeline input = ByValue.
3. ByValue is the first method that PowerShell uses to move commands across the
pipeline the second method is called ByProperty.

---

As you already know pipeline parameter binding is the method of stringing together the right cmdlets to perform a task.
`Byvalue or Byproperty is the glue that PowerShell uses to allow the output of one command to pass to the input of the second command`

In the last lecture we demonstrated how to get commands through the pipeline Byvalue.
In this lecture, we’ll demonstrate how PowerShell uses parameter binding byproperty.

We’ll introduce two new commands:
• `import-csv and the new-alias command.`
• The Import-Csv cmdlet creates table-like custom objects from the items in
CSV files. Each column in the CSV file becomes a property of the custom
object and the items in rows become the property values
• The New-Alias cmdlet creates a new alias in the current PowerShell session.
let’s Open windows explorer. From your c: drive create a folder named test.
Now we’ll use notepad to create a csv (Comma separated value) file and copy that to our test folder.
• In notepad type the following:
```
Name, Value
L, eventlog
List, get-childitem
P, ping
W, winver
```
Now from file, click save-as. Click the C: drive and click the test folder.
For a file name type aliases.csv
Change save as type, press the down arrow and change .txt to All Files.

Now, the parameter binding process that PowerShell uses to determine what commands pass through the pipeline is basically the same for byproperty and for byvalue, with a few differences.
For command #1 you still use get-member and for command #2 you still use get-help. The
difference is that now you are looking for things that are byproperty instead of byvalue.
• Command 1 which in this case is import-csv. We’ll use get-member to get the property
and methods of the object import-csv.
• Command 2 will be New-Alias. We’ll us get-help –full to get the parameters that take
byproperty.
• Now we’ll open two PowerShell windows.
```
import-csv -path c:\test\aliases.csv | gm
```
If you recall from our object’s lecture. Objects can have properties or methods. But what we’re
interested in is properties and in this case Powershell displays them as noteProperty
• (NoteProperties are just generic properties that are created by Powershell)
• Notice that Name and Value match the column headings from our csv file.
Now we need to use get-help to determine if Command 2 or in this case New-alias has a
parameter that will bind with command 1 ByProperty
```
get-help new-alias -full
```
• Scroll up until you see a parameter that says ByPropertyName
• So, we see that the -Value and -Name parameters both accept pipeline input
BypropertyName
These two commands should work because:
Command one which is import-csv passed two properties called name and value across the
pipeline to the second command which is new-alias.
From the second command new-alias Get-help told us that the parameters -name and -value
can take pipeline input by propertyname
So, these command should work. We can test that by typing
```
 import-csv -path c:\test\aliases.csv | new-alias -verbose
```
now let’s check out our new aliases
• Type L security and our security log is displayed
• Type list and the files on the root of my C: drive are displayed
• Type P 127.0.0.1 and we can test our loopback.
• Press w and my current version of windows is displayed.


**4)** For the next byproperty example we’ll need to create another csv file.
So let’s go ahead and open notepad again. We’ll use the same process as we did before.
```
svcname, svcstatus
Bits, running
```
• Now for a file name type bits.csv and save this to the test directory as well.
This time we’ll just run the command.
```
import-csv -path c:\test\bits.csv | stop-service
```
• `And we see that the command failed, but why?`
• The error log says that stop-service Cannot find any service with service name
'@{svcName=bits; SvcStatus=running }'.

We’ll open two powershell windows again
First we’ll check command 1, using get-member.
 ```
import-csv -path c:\test\bits | gm
```
• Notice svcName noteproperty and string (which in this case is a text string)
Now lets check the full help on the second command

```
get-help stop-service -full
```
• Scroll up until you see a parameter that supports a string.
And we see that -Name supports a string
it also accepts pipeline input ByPropertyName and ByValue
• Ok now open our bits.csv file.
• Open file explorer, click C:\test, right click and choose edit on the bits.csv file.
all we have to do is to rename our column header from scvname to name.
`The column header determines the names of the properties of the object that Importcsv
creates and in this case are passed across the pipeline to our second command
which is stop-service`
• Go ahead del svc and then click save and close notepad.
• Now run the command again, in this case go ahead and add the parameter -passthru
```
import-csv -path c:\test\bits.csv | stop-service -passthru
```
• And you see that the status of bits is now stopped, so you can see that the command
ran successfully.

• The only parameter that stop-service (our second cmd) would take bypropertyname
was -name.
• Import-csv (1st Cmd) tried to push SVCname across the pipeline to stop-service. But
stop-service will only match up with -name not svcname.
• After we modified the column header inn notepad from SvcName to the property,
Name, Stop-service was able to use the parameter -name as a match and the
command ran.