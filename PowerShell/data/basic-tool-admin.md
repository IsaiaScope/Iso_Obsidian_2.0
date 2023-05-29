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