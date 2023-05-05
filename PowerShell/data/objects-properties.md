- object concept is the same as OOP

Let’s take a look at some examples using Methods:

We’re going to be using get-process and the process named notepad
• First let’s open notepad, open your search bar and press notepad, minimize to the taskbar
• Type get-process, press return, press return again. Scroll up, and we see that notepad is running. Press return and cls.

```
get-process -name notepad
```
when we take a look at get-process there are no methods or properties listed. So, the question is, how can I display the property or methods for get-process? By using a special command called get-member. We’ll use the alias gm for get-member.
```
get-process -name notepad | gm
```
N.B. (Use pipe operator). If you recall, a pipeline takes the output of one command and pushes it through to the input of the second command.
So, get-process has a process called notepad, and you’re piping the output of notepad into the input of get-member.





