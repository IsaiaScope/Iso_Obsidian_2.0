---
tags:
---

# accessible name


Whenever we try to run the get viral function, the expectation is that we are going to find exactly

one element.

So in this case, if I try to get by row with button, I'm going to find two buttons and the result

is we get an error message.

So in this particular case it says we found multiple elements with the same role.

Here are the two that have that role.

So again, get buy roll is going to give us an error if we ever find anything other than one element.

So we need to be a little bit more specific in this case.

We need to add in a little bit more of a filter criteria to this query function, and we need to say

exactly which button element we are trying to find.

One possible way to do this is by using the accessible name of an element.

So accessible name is a technical term of sorts.

The accessible name of an element is going to be the text within it.

In some cases, some elements do not have text inside of it.

For example, input elements an input.


![[Pasted image 20240315115535.png]]

good approach 
![[Pasted image 20240315120538.png]]


---

Just a moment ago, I told you that the accessible name is going to be whatever text we put in between

these two tags, but in some cases, like input elements, we are not really supposed to make two separate

open and close inputs and put text in between.

Instead, inputs are really intended to be self-closing, something like this.

So in this case, how would we use an accessible name with an input?

Well, let me show you that really quickly.

This is something that we've seen on our previous application work done, but I just want to kind of

document this in the notebook.

So you've got a record of it going into the future down here at the very bottom of the page, I'm going

to add in another code cell.
![[Pasted image 20240315120732.png]]

![[Pasted image 20240315120830.png]]

solution

![[Pasted image 20240315121023.png]]


![[Pasted image 20240315121052.png]]


##  Directly Assigning an Accessible Name

