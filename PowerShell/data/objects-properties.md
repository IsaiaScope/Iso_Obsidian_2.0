- object concept is the same as OOP

**1)** Let’s take a look at some examples using Methods:

We’re going to be using get-process and the process named notepad
- First let’s open notepad, open your search bar and press notepad, minimize to the taskbar
- Type get-process, press return, press return again. Scroll up, and we see that notepad is running. Press return and cls.

```
get-process -name notepad
```
when we take a look at get-process there are no methods or properties listed. So, the question is, `how can I display the property or methods for get-process? By using a special command called get-member. We’ll use the alias gm for get-member.`
```
get-process -name notepad | gm
```
N.B. (Use pipe operator). If you recall, a pipeline takes the output of one command and pushes it through to the input of the second command.
So, get-process has a process called notepad, and you’re piping the output of notepad into the input of get-member.

There is some very important information, when using get-member.
It’s TypeName. **Typename** tells us what kind of object that is being sent across the pipeline. In this case the TypeName is System.Diagnostics.Process but we’ll just use the last part which is process. Just keep that in mind, you’ll need it later when we get to the pipeline lecture.

 - Notice the method called kill. Kill is definitely an action.
```
(Get-Process Notepad).kill()
```
`To use a method of an object, Place the cmdlet name and the argument in parenthesis, then type a dot (.), then the method name, and a set of parentheses "()".
The parentheses are required for every method call, even when there are no arguments.
The point is that we don’t need a separate cmdlet to close the process notepad we
have the method kill.
And we see that notepad has been closed.`

**2)** Let’s try another example of using Methods

Let’s say we want to copy files from one location to another. In this case we’ll use the
get-childitem command.

Let open windows explorer,and create a folder named content with computer.txt file inside
Double click on content and we have a text file named computers.
- Now we want to copy this file from the C:\content folder to the c:\test folder using a
method called copyto.
```
 get-childitem | GM
```
Notice the get-childitem TypeName: is FileInfo
We’ll use the copyto method to copy a file from one location to another

- We don’t need a cmdlet, not when we have the copyto method, press return
Let’s go ahead and type the command then I’ll explain it.
```
 (Get-childitem C:\content\computers.txt).copyto("C:\test\computers.txt")
```
Put the cmdlet name and the argument in parenthesis then type a dot (.), the method
name, in this case is copyto. and a set of parentheses "()".
If the method has arguments, place the argument values inside the parentheses.
In this case we also added the quotation marks, and press return
And we see that the copyto method was successful. Go out to explorer and the test
folder, and there is our text file.
This was just two examples of using methods, but it gives you an idea of how to use
methods to accomplish an everyday task.

**3)** Now let’s take a look at using the Properties of an object

Let’s use the get-childitem command. 
```
 get-childitem | gm
```
We want to use the `creationtime` property to find out the creation date of the current version of PowerShell
```
(Get-ChildItem $pshome\PowerShell.exe).creationtime
```
- The most common way to get the values of the properties of an object is to
use the dot method. That means that you first surround the parameter and the path with parenthesis. Then insert a (.) then the property. Which in this case is creationtime.
And we get Wednesday, April 11, 2018 07:35 PM
- By the way $pshome is the path to the PowerShell home folder

**3.1)** Another way to get the properties of an object is to use the select-object command. The select-object command has a parameter called –property that will get the properties of an object.

```
Select-object -showwindow
```
- The parameter that we are going to be using is called -property.
Select-object is the name of the cmdlet.
Notice that the parameter -property and the argument’s value type - called object,
both are surrounded by square brackets. That means that both parameter and
argument are optional and not needed.
Notice also that -property is surrounded by a separate set of square brackets so -
property is positional as well.
We can verify that by scrolling up and looking at the parameter attributes. Which tell us
that -property has a position of 0, and it is not required.
That tells us that -property should be located in the first position in the lineup of
parameters.
We can also see that the argument has two square brackets inside the two angle
brackets this means that the parameter -property can take multiple arguments
separated by a comma.

We’ll use `get-eventlog security and select-object` for this demonstration
Instead of displaying the whole security log let’s just display the newest 6 events.
```
Get-eventlog -logname security -newest 6
```
- Now let’s use Get-member, which will show us the properties and methods
```
Get-eventlog -logname security -newest 6 | get-member
```
- Let’s select a few useful properties. How about Time-Generated, EventID and
machinename
```
Get-eventlog -logname security -newest 6 | Select-object Source, TimeWritten,
machinename, Message
```
In case your’e wondering why I used -logname, because as we discussed earlier it’s
optional and not needed. Here’s why, when your’e first starting out in PowerShell, you
might want to go ahead and type out some of the optional parameters. Especially if you
plan on saving your commands and one liner’s as scripts for later use. It just makes it
easier to remember what these commands and parameters are doing if you go ahead
and type them out. The same goes for aliases you can use gsv for get-service. But it’s a
whole lot easier to remember what get-service -name BITS is doing instead of gsv bits.
Both commands will work, but when you’ve type out the whole command it’s easier to
understand especially when you’re first starting with PowerShell.

*Questions*
- [[Obj-prop-methods-es-1.pdf]]
- [[Obj-prop-methods-es-2.pdf]]