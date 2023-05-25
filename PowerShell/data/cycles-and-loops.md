- **for**

#For loop allows you to repeat a section of code a specific number of times
#for (Define the variable we want to use to count and its value; what kind of condition we will use when were counting: either increase or decres the variable) (
     a Command we want to repeat
#)
```
$MyArray = @("Cars", "Trucks")
for ($i=0; $i -lt $MyArray.Count; $i++) {
 echo ("Element $i value: " + $MyArray[$i])
}
```

- **while**
```
while($userInput -ne "quit") {
	# Gather user input
	$userInput = Read-Host "Say 'yes' if you like scripting (enter 'quit' to stop the loop)"
	# Evaluate user input
	if($userInput -eq "yes") { echo "I also love scripting! That's cool" }
	elseif($userInput -eq "no") { echo "I really hate scripting too" }
}
$userInput = $null
```
- **do**
```
$i=0
do {
    $i++
 } until ($i -eq 5)
echo "Value of i = $i"
```
- **forEach**
classic for
```
$Vehicles =  @("Motorcycles", "Trucks", "SUVs")
for ($i=0; $i -lt $Vehicles.Count; $i++) {
	echo ("Element $i = " + $Vehicles[$i])
}
```
with foreach
```
$Array = @("Element1", "Element2")
ForEach ($Element in $Array) {
 $Element
}
```
```
$People = @("Paul Hill", "Jason Tolber", "Charlie Irkins", "Malanie Garth")
ForEach ($Person in $People) {
	echo "Creating new Active Directory User Account for $Person"
}
```

- **break & continue**
```
$array = @("Honda", "Toyota", "Ford", "Chevy")
foreach ($element in $array){
 if ($element -eq "Ford") { break }
 $element
}
```
```
$array = @("Honda", "Toyota", "Ford", "Chevy")
foreach ($element in $array){
 if ($element -eq "Ford") { continue }
 $element
}
```