1. [semantics_in_html](https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html)
2. [semantics-builtin](https://web.dev/semantics-builtin/)
3. https://developer.mozilla.org/en-US/docs/Learn/Accessibility/WAI-ARIA_basics
4. https://github.com/Stefany93/accessible-to-do-list
5. https://github.com/Stefany93/furbaby-accessible-website
![[to-delete.png]]

aria-expanded (boolean) => to tell to screen reader if is open or close
<button id="hamburger" aria-expanded="false">


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
outline

