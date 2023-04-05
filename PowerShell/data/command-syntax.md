```js
Get-EventLog 
	[-LogName] <System.String> 
	[[-InstanceId] <System.Int64[]>] 
	[-After <System.DateTime>] 
	[-AsBaseObject] 
	[-Before <System.DateTime>] 
	[-ComputerName <System.String[]>] 
	[-EntryType {Error | Information | FailureAudit | SuccessAudit | Warning}] 
	[-Index <System.Int32[]>] 
	[-Message <System.String>] 
	[-Newest <System.Int32>] 
	[-Source <System.String[]>] 
	[-UserName <System.String[]>] 
	[<CommonParameters>]
    
Get-EventLog 
	[-AsString] 
	[-ComputerName <System.String[]>] 
	[-List] [<CommonParameters>]
```
----
**Intro** 
`help get-eventlog -showwindow`

**Command Structure**
Every Cmdlet in PowerShell follows this basic structure: 
`Verb-Noun -param1 -param2 separated by a comma.` 
What are all these brackets, hyphens, angle brackets, braces all about? I’ve already shown you what parameters are. If you recall parameters always start with a hyphen. 
Parameters are options that describe what the cmdlet will do. 

**Parameter Sets** 
If you see a command name more than once, that means that there'll be at least one unique parameter in each set. 
Notice in the first set there are several unique parameters such as -logname, -instanceId, -After, -Before, -Newest and several others. 
In the second set you have -AsString and -List.
Notice also the -computername is parameter listed in both sets. That means you can use the -computername parameter when using either set.

In our example type get-eventlog -logname application, we’re using the first parameter set.
Now try and use a parameter from the second set. 
Try using -list, You’ll see -list is not there. If you type -list and press return. You’ll get an error.
So, what I am showing you is that `if you start using the parameters from one set you can’t jump over and use parameters from the other set`. Those parameters won’t work. 

**Arguments <>**
The angle brackets indicate an argument. 
What’s inside the angle brackets is called a value type. 
In our example we type get-eventlog, space dash logname. The logname parameter has a value type called which can be an alpha-numeric value. 
In this case it’s a text string such as a single word like 'security' or 'application'.

**Multiple Arguments []**
Notice the parameter -InstanceID and the argument <Int64[]>. int stands for integer. See the two square brackets? 
Note the square brackets are inside the angle brackets. 
This means that it can take multiple arguments separated by a comma. 
like => -instanceID now type 0,1. 

Our command is get-eventlog, our first parameter is -logname, the angle brackets indicate an argument. Inside the angle brackets is a value type and the value type is string which in this case is a text string. We chose the application log. The next parameter is – InstanceID which takes an argument < > the value type is <Int64> which refers to an integer or numeric. Again, notice that within the angled brackets are two square brackets. This means that -instanceID can take multiple arguments. In this case I’ll type 0,1 separated by a comma.

Note: Not all parameters can take multiple arguments, you will always want to check this. For example, type get-eventlog -logname security -newest 3,2 press return, and you get an error. 
Checkout the syntax for the parameter -newest. It can take an argument but not multiple arguments. Notice there are not two square brackets within the angle brackets. This shows that -newest can only take a single argument.

**How do you tell which Parameters are optional and which arguments are required?**
To answer that question we need PowerShell’s Help system Type `help get-service -showwindow`

notice that this cmdlet can't run without adding any parameters.  

`[Parameter Argument] Notice that all the parameters and the arguments are surrounded by square brackets. This means that adding the parameters are optional and not needed.`

`notice that almost every parameter and every argument are surrounded by square brackets, that means that they are all optional or not needed. `
`Notice -logname is surrounded by square brackets but the argument is not. Required Argument That means, because there are square brackets around the parameter -logname, the name logname is optional but the argument is required.`
`That is why when you ran get-eventlog without any parameters, PowerShell asks for a value for -logname`

**Positional parameters **
-InstanceID, -logname and -newest 

-logname has a position of 0, 
-InstanceID has a position of 1 
-newest has a position of named. 

Now what does this mean. 0, 1 and name, refer to the actual position that the cmdlet must be placed in the order of the cmdlets. 
So -logname is positional, it’s position is 0 which is the first position. 
The parameter -instanceID position is 1 which is after -logname in the order of cmdlets 

Ok lets check that out So type `get-eventlog application 0,1` (I don’t have to type the parameter -logname or the parameter -InstanceID, because they are both optional because the’re surrounded by square brackets 
Now take a look at the argument for -InstanceID because there are two square brackets 3 within the angle brackets the parameter -InstanceID can take multiple arguments. 

Now lets see if we can move these values out of order. Move the 0,1 in front of application and press return. An we get an error because Powershell expects the positional parameter -logname or the value type application to be the first in the list. 

**Named Parameters**
Take a look at -Newest, notice that the position is named. Named means that you can put -newest anywhere in the order of parameters and it will work. Let’s check it out Type `get-eventlog -newest 5 application 0,1` and that worked. We see that we moved the parameter with a position called named and moved that in front of the positional parameter -logname and we see that the command ran

**{} Curly braces** 
In some of the commands you’ll see curly braces. 

-Entrytype. Notice that it is surrounded by square brackets that start at -EntryType and end at warning} ]. 
That shows that the parameter is optional. 
But notice the curly braces surrounding {Error and ending at warning} 
Also notice the vertical lines between information | FailureAudit | SuccessAudit and Warning. 
What this means is, that if you want to use the parameter -Entrytype you have these choices. 
Let’s try this Type `Get-EventLog application -EntryType warning, error -Newest 20`.
This command displays any warnings and errors coming from our applications that we are currently running.

**Required Parameters**
There’s One thing that I need to show you. 
Let go back to the syntax for get-service. 
Take a look at the first parameter set, notice that the parameter -displayname doesn’t have the square brackets around it. This means that if you want to use the first parameter set, you would be required to use the parameter -DisplayName. That’s how you know what is required and what is optional. 
Type `get-service -displayname and pick an application` press return, and as you can see that did command run. This command displays the service that is associated with the application. 

`So, in most instances this rule would hold true. If there are no square brackets around the parameter and the argument, the parameter and the argument are required. If there are square brackets around the parameter and the argument the parameter is optional and not required. `

---

**Let’s go back to the get-evenlog syntax and review:** Get in the habit of analyzing the syntax of a cmdlet 
1. – indicates a parameter 
2. < > angle brackets indicate an argument 
3. [] If there are two square brackets inside two angle brackets this means that the parameter can take multiple arguments, separated by a comma. In this case it would be two numbers. 
4. [Param Arg] Square brackets around the parameter and the argument. Means the parameter is optional. [Param] Square brackets around the parameter 
	 Because -logname is surrounded by square brackets and the argument is not, that makes -logname optional and the argument required. 
5. Positional Because -logname is surrounded by square brackets and it’s position is 0, this makes this parameter positional. So, PowerShell expects -logname and it’s value type to be first in the order of cmdlets. 
6. Named – A parameter that has a position of named can be placed anywhere in the order of cmdlets. 
7. { } Curly braces - a parameter followed by several choices separated by vertical lines and surrounded by curly braces. Using this parameter, you can choose various items to expand the functionality of the cmdlet. 
So, with what you have learned in the last two lectures you should be able to figure out command syntax. You’re on your way to understanding a lot more about PowerShell.



