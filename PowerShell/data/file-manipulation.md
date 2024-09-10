1.  file select dialog
 ```
# Load necessary assemblies

Add-Type -AssemblyName System.Windows.Forms

  

# Create OpenFileDialog object

$openFileDialog = New-Object System.Windows.Forms.OpenFileDialog

  

# Set OpenFileDialog properties (optional)

$openFileDialog.Title = "Select a file"

$openFileDialog.Filter = "All Files (*.*)|*.*|Text Files (*.txt)|*.txt|CSV Files (*.csv)|*.csv"

$openFileDialog.InitialDirectory = "C:\"

  

# Show the OpenFileDialog and get the result

$dialogResult = $openFileDialog.ShowDialog()

  

# Check if the user clicked 'Open' (OK)

if($dialogResult -eq "OK") {

    $selectedFile = $openFileDialog.FileName

    Write-Host "You have selected the following file: $selectedFile"

} else {

    Write-Host "No file was selected."

}
```
2. csv manipulation
```
# Load necessary assemblies

Add-Type -AssemblyName System.Windows.Forms

  

# Create OpenFileDialog object

$openFileDialog = New-Object System.Windows.Forms.OpenFileDialog

  

# Set OpenFileDialog properties (optional)

$openFileDialog.Title = "Select a file"

$openFileDialog.Filter = "All Files (*.*)|*.*|Text Files (*.txt)|*.txt|CSV Files (*.csv)|*.csv"

$openFileDialog.InitialDirectory = "C:\"

  

# Show the OpenFileDialog and get the result

$dialogResult = $openFileDialog.ShowDialog()

  

# Check if the user clicked 'Open' (OK)

if($dialogResult -eq "OK") {

    $selectedFile = $openFileDialog.FileName

    Write-Host "You have selected the following file: $selectedFile"

  

    # Import CSV Data

    $csvData = Import-Csv -Path $selectedFile

  

    # Inspect data with OGV

    $csvData | Out-GridView

  

    # Get new user info

    $firstName = Read-Host "Enter the new user's first name"

    $lastName = Read-Host "Enter the new user's last name"

  

    # Create Power Shell Custom Object with new user info

    $newUser = [PSCustomObject] @{

        FirstName = $firstName

        LastName = $lastName

    }

    # TODO Add user to array

    $csvData += $newUser

    # Save data back to CSV

    $csvData | Export-Csv -Path $selectedFile -NoTypeInformation -Force

  

    # Output to the console

    Write-Host "New user $firstName $lastName was successfully added to the CSV File"

  
  

} else {

    Write-Host "No file was selected."

}
```
3. json manipulation
```
# Load necessary assemblies

Add-Type -AssemblyName System.Windows.Forms

  

# Create OpenFileDialog object

$openFileDialog = New-Object System.Windows.Forms.OpenFileDialog

  

# Set OpenFileDialog properties (optional)

$openFileDialog.Title = "Select a file"

$openFileDialog.Filter = "All Files (*.*)|*.*|Text Files (*.txt)|*.txt|JSON Files (*.json)|*.json"

$openFileDialog.InitialDirectory = "C:\"

  

# Show the OpenFileDialog and get the result

$dialogResult = $openFileDialog.ShowDialog()

  

# Check if the user clicked 'Open' (OK)

if ($dialogResult -eq [System.Windows.Forms.DialogResult]::OK) {

    $selectedFile = $openFileDialog.FileName

    Write-Host "You have selected the following file: $selectedFile"

  

    # Import JSON Data

    $jsonData = Get-Content -Path $selectedFile | ConvertFrom-Json

  

    # Inspect data with Out-GridView

    $jsonData | Out-GridView -Title "Current JSON Data"

  

    # Get new user info

    $firstName = Read-Host "Enter the new user's first name"

    $lastName = Read-Host "Enter the new user's last name"

  

    # Create PowerShell Custom Object with new user info

    $newUser = New-Object -TypeName PSObject -Property @{

        FirstName = $firstName

        LastName  = $lastName

    }

  

    # Add user to array

    $jsonData += $newUser

  

    # Save data back to JSON File

    $jsonData | ConvertTo-Json | Set-Content -Path $selectedFile

  

    # Output to the console

    Write-Host "New user added successfully to the JSON file:"

    $jsonData | Format-Table

  

} else {

    Write-Host "No file was selected."

}
```
---
**File Manipulation Assignment**

#### Objective

In this assignment, you will practice your skills in working with OpenFileDialog, CSV file manipulation, and JSON file manipulation. You will create a script that allows users to select a CSV or JSON file containing user data, add a new user, and save the updated data back to the file.

#### Requirements:

1. Create a script that presents an OpenFileDialog to the user, allowing them to select a CSV or JSON file containing user data.
    
2. Ensure that the OpenFileDialog only shows CSV and JSON files.
    
3. Read the selected file and import the user data, taking into account whether the file is a CSV or JSON file.
    
4. Display the existing user data in a table format to the console.
    
5. Prompt the user to enter the first name and last name of a new user.
    
6. Add the new user to the existing user data.
    
7. Save the updated user data back to the selected file, taking into account whether the file is a CSV or JSON file.
    
8. Display the updated user data in a table format to the console.
    

#### Instructions:

1. Use the `**System.Windows.Forms.OpenFileDialog**` class to create an OpenFileDialog to select a CSV or JSON file.
    
2. Load the necessary assemblies for working with Windows Forms.
    
3. Set the appropriate file filters for the OpenFileDialog.
    
4. Check the user's action and proceed if the user selected a file.
    
5. Import the user data from the selected file. Use `**Import-Csv**` for CSV files and `**ConvertFrom-Json**` for JSON files.
    
    1. HINT: You can use $selectedFile.contains('.csv') and $selectedFile.contains('.json') to see what kind of file type they selected
        
6. Display the existing user data to the console using `**Format-Table**`.
    
7. Prompt the user for the first name and last name of a new user using `**Read-Host**`.
    
8. Create a PowerShell custom object for the new user and add it to the existing user data.
    
9. Save the updated user data back to the selected file. Use `**Export-Csv**` for CSV files and `**ConvertTo-Json**` and `**Set-Content**` for JSON files.
    
10. Display the updated user data to the console using `**Format-Table**`.