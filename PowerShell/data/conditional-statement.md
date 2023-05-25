- **if**
```
$MyVar1 = 1
$MyVar2 = 2
if ($MyVar1 -eq $MyVar2) {
    echo "I like hotdogs"
} else {
	echo "I dislike hotdogs"
}
```
- **elseif**
```
Schoice - Read-Host "Please select an option (1-3)"
if (Schoice -eq 1) (
     echo "You chose option 1"
) elseif (Schoice -eg 2) (
     echo "You chose option 2"
) elseif (Schoice -eg 3) {
     echo
          "You chose option 3"
) else {
    echo "You did not chose a valid option"
)
 # Switch statement
switch (Read-Host "Please select an option (1-3)") (
     1 ("You chose option 1")
     2
      ("You chose option 2")
     3 {"You chose option 3")
     default ("You did not chose a vlid option")

```
- **switch**
```

```