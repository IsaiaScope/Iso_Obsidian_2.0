- **if**

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