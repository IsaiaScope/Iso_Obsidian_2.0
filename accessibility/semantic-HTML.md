1. [semantics_in_html](https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html)
2. [semantics-builtin](https://web.dev/semantics-builtin/)
3. https://developer.mozilla.org/en-US/docs/Learn/Accessibility/WAI-ARIA_basics
4. https://github.com/Stefany93/accessible-to-do-list
5. https://github.com/Stefany93/furbaby-accessible-website
![[to-delete.png]]

aria-expanded (boolean) => to tell to screen reader if is open or close
```html
<button id="hamburger" aria-expanded="false">
```



---
aria-label => <svg> insie <a> insert aria-label in svg to describe the svg icon
<a href="https://www.instagram.com"> <svg version="1.1" aria-label="Follow company on Instagram"  xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w.org/1999/Xlink" width="32" height="32" viewBox="0 0 16 16">*</svg> </a>
---
aria-labelledby => 
<img src="https://images.unsplash.com/photo- 1563046937-9824d5725660?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb&ixid= eyJhcHBfawQiojEoNTg5 fQ" alt="" aria-labelledby="caption" width="200"> <p id="caption"›Captions</p› 
---
role => change an elemet into another, NEEDS TABINDEX to be interactive
---
img 
https://webaim.org/techniques/images/ accessibile img
use alt or labelledby; NOTE leave alt empty to guarantee accessibility 
<figure>
<img src="https://images.unsplas h.com/photo-1489345745021- 740d36bbda21?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entr opy&cs=srgb&ixid=eyJhcHBfawQiOjEONTg5fQ" alt="" aria- labelledby="caption" width="400" height="200"> <figcaption id="caption"> A white cotton shirt. </figcaption></figure>

or 

<img src="https://images.unsplas h.com/photo-1554597998- 97267a7?bob?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entr opy&cs=srgb&ixid=eyJhcHBfawQiojEONT8sfQ" alt="Dog says 'I can make it to the fence in 2.8 seconds. Can you?'" width="400" height="400">

---
outline must have
focus and hover same style


---

tabindex  adds interactivity to the element
value 0 doesn't put the element in order make that just 'tabbable' 
value -1 removes the elm from 'tabbable'  group but still focussable with js
value 1 to x is the tab order of focussing elms

---

bypass block (skip links)
**Bypass blocks** are mechanisms that skip over repeated material on a webpage. They are important for users who navigate with a keyboard because as they will allow users to skip over repeated sections and go to the content they are looking for immediately.
The example below is a simple example of how a skip navigation functions. The “skip to main content” link will skip over the list of websites and jump to the main content of the page instead. This allows for a keyboard user to bypass the section and get to the main information right away instead of having to go through every single link before getting to the main information.
CSS is added to make the skip navigation link invisible until it gains keyboard focus. Once it gains focus it becomes visible to a sighted user and will also be read by a screen reader user. The CSS used sets the link at the top left corner of the page. Once tabbed into, it will gain focus and be visible to sighted users. It will also be read by screen readers.
<body>
    <div id = "skip-to-main">
        <a href = "#main-content">Skip to main content</a>
    </div>
    <nav>
        <ol>
            <li><a href = "https://www.google.com" target = "_blank">Google</a></li>
            <li><a href = "https://www.youtube.com" target ="_blank">Youtube</a></li>
            <li><a href = "https://www.webaim.org" target = "_blank">WebAIM</a></li>
            <li><a href = "https://www.csun.edu" target ="_blank">CSUN</a></li>
        </ol>
    </nav>
 
    <main id = "main-content">
            <h1> Welcome!</h1>
            <div tabindex = "0">How is your day going?</div>
    </main>
</body>

#skip-to-main a{
	position: absolute;
	left: -10000px;
	top: auto;
	width: 1px;
	height: 1px;
	overflow: hidden;
}

#skip-to-main a:focus{
	position: static;
	width: auto;
	height: auto;
}


---
tags u need
nav
ul 
li
a

https://www.smashingmagazine.com/2017/11/building-accessible-menu-systems/


---
humburger


https://codepen.io/Stefany93/post/good-hamburger-navigation