# How Do You Find Your Current User Agent in Safari?

> https://www.browserstack.com/guide/safari-browser-user-agent

## Method 1 ➜ Utilising Developer Tools on macOS

- _Launch Safari_ ➜ Open the Safari application on your Mac.

- _Enable the Develop Menu_ (if it is not already activated) ➜ To access the preferences in Safari, click on the “Safari” option in the menu bar and then select “Preferences”. Select the `Advanced` tab. Select the checkbox labelled “Show Develop menu in the menu bar”.

- _Access Developer Tools_ ➜ Navigate to the menu bar and select `Develop`. Choose the option _Show Web Inspector_ or use the shortcut `Option + Command + I`.

- _Inspect User Agent_ ➜ Navigate to the `Network` section in the Web Inspector. Refresh the webpage. Select the initial request (often the primary document request). Please navigate to the `Headers` tab and locate the `User-Agent` string within the `Request Headers` section.

## Method 2 ➜ Utilising Internet Resources

- _Launch Safari_ ➜ Open Safari on your Mac or iOS device.
- _Access a User Agent Detection Website_ ➜ Access third-party websites such as [WhatIsMyBrowser.com](http://whatismybrowser.com/ "WhatIsMyBrowser.com") or [UserAgentString.com](http://useragentstring.com/ "UserAgentString.com") which help in detecting and verifying current browser or user agent string.

## Method 3: Utilising JavaScript

- _Initiate Safari_ ➜ Start Safari on your Mac or iOS device.
- _Access the Console_ ➜ On macOS, open the console by going to Develop ➜ Show JavaScript Console or press `Option + Command + C`.
- _Input JavaScript Command_ ➜ Type `navigator.userAgent` into the console and press Enter.
- _Display User Agent_ ➜ The console will show the current user agent string.

---

# How to Change the Safari Browser User Agent? 

## To change the Safari browser user agent on macOS, you can use the Develop menu.

1. Launch Safari on your Mac.

2. Enable the *Develop Menu* (if not already enabled):

   - Go to *Safari ➜ Preferences* from the menu bar.
   - Click on the *Advanced tab*.
   - Check the box that says *Show Develop Menu* in the menu bar.

3. Change the User Agent:

   - In the menu bar, you will now see a *Develop* menu.
   - Click on *Develop*.
   - Hover over *User Agent* to see a list of predefined user agents.
   - Select the user agent you want to use from the list.

4. Options include:

   - Presets
   - If you need to enter a `custom` user agent string, select `Other`… at the bottom of the User Agent submenu, then enter the desired user agent string in the dialog box and click OK.

_NOTE_ `If you require a permanent user agent for several sessions and websites, it is advisable to use a browser extension`. However, it should be noted that Safari’s available alternatives are more limited than those of other browsers such as Chrome or Firefox. `Safari’s built-in features do not explicitly allow for lasting changes to the user agent`.

---
