KB Patch Retriever
This script returns the installed KB patches on a Windows computer.
Go ahead and copy and Paste the command line from the student guide
Here is the Command Line:
```
$wu = new-object -com “Microsoft.Update.Searcher”
$totalupdates = $wu.GetTotalHistoryCount()
$all = $wu.QueryHistory(0,$totalupdates)
$OutputCollection= @()
Foreach ($update in $all)
{
$string = $update.title
$Regex = “KB\d*”
$KB = $string | Select-String -Pattern $regex | Select-Object {$_.Matches }
$output = New-Object -TypeName PSobject
$output | add-member NoteProperty “HotFixID” -value $KB.‘ $_.Matches ‘.Value
$output | add-member NoteProperty “Title” -value $string
$OutputCollection += $output
}
$OutputCollection | Sort-Object HotFixID | Format-Table -AutoSize
Write-Host “$($OutputCollection.Count) Updates Found”
```
Here is the Explanation:
1. $wu = new-object -com "Microsoft.Update.Searcher": This line creates a new COM object for the Windows Update searcher using the new-object cmdlet. The COM object or a ( Component Object Model ) is stored in the $wu variable. (A com object is a mechanism for Powershell to launch programs. This object is used to search for Windows updates.
2. $totalupdates = $wu.GetTotalHistoryCount(): This line retrieves the total number of update events in the update history using the GetTotalHistoryCount method of the $wu object. The count is stored in the $totalupdates variable.
3. `$all = $wu.QueryHistory(0,$totalupdates)`: This line retrieves all of the update events in the update history using the QueryHistory method of the $wu object. The results are stored in the $all variable.
4. $OutputCollection= @(): This line creates an empty array called $OutputCollection that will be used to store the output of the script.
5. Foreach ($update in $all): This line starts a loop that iterates through each update event in the $all variable.
6. $string = $update.title: This line retrieves the title of the update event and stores it in the $string variable.
7. $Regex = "KB\d*": This line creates a regular expression pattern that matches the format of a KB patch.
8. `$KB = $string | Select-String -Pattern $regex | Select-Object {$_.Matches }: This line uses the Select-String cmdlet to search for the KB patch ID in the $string variable using the $regex pattern. The Select-Object cmdlet is used to retrieve the match object and store it in the $KB variable.`
9. $output = New-Object -TypeName PSobject: This line creates a new object called $output that will be used to store the results of the loop.
10. $output | add-member NoteProperty "HotFixID" -value $KB.' $_.Matches '.Value: This line adds a new member to the $output object called "HotFixID" with the value of the KB patch ID that was found in the $string variable.
11. $output | add-member NoteProperty "Title" -value $string: Add a new note property named "Title" to the $output object with the value of the update's title.
12. $OutputCollection += $output: This line adds the $output object to the $OutputCollection array.
13. } End of the loop.
14. $OutputCollection | Sort-Object HotFixID | Format-Table -AutoSize: This line sorts the $OutputCollection array by the "HotFixID" member and formats the output as a table with auto-sized columns.
15. Write-Host "$($OutputCollection.Count) Updates Found": This line writes a message to the console indicating the total number of KB patches that were found in the update history.
Let’s go ahead and run the script, and take a look at the output. In this case, there were 123 updates found.
This script essentially retrieves the entire update history of a Windows system, processes each update to extract the KB article number and title, and then sorts and formats the information in a table for better readability.