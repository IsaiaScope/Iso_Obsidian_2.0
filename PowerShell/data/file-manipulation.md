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

```