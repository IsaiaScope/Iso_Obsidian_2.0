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
```
3. json manipulation
```
```