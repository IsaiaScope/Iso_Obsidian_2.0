- [DOC of Semantics HTML at MDN](https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html)
- [DOC of Introduction to semantics](https://web.dev/semantics-builtin/)
- [DOC of WAI-ARIA basics](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/WAI-ARIA_basics)
- [GitHub Repo of furbaby-accessible-website](https://github.com/Stefany93/furbaby-accessible-website), repo where most of theory code is taken

_IMPORTANT CONCEPTS _

- using semantic html don't need to specify the element inside aria-labels because screen readers are going to tell that anyway the current tag
- outline must be displayed
- focus and hover same style
- [[ARIA | Link to ARIA Note]]

---

_ROLES_

- The role attribute can be used to mimic a HTML element
- change an elemet into another, _NEEDS TABINDEX to be interactive_
- Again, _we want to use semantic HTML first, and this as a last resort_
- role="navigation"
- role="checkbox"
- role="text" (not part of specification)
- Avoid redundancy

```html
<nav role="navigation"></nav>
```

- [[role.png.png | IMG of role example]]

---

_IMG_

- [DOC of Accessible Images](https://webaim.org/techniques/images/)
- use alt or labelledby; _NOTE_ leave alt empty to guarantee accessibility

```html
<figure>
	<img
		src="https://images.unsplas h.com/photo-1489345745021- 740d36bbda21?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entr opy&cs=srgb&ixid=eyJhcHBfawQiOjEONTg5fQ"
		alt=""
		aria-
		labelledby="caption"
		width="400"
		height="200"
	/>
	<figcaption id="caption">A white cotton shirt.</figcaption>
</figure>
```

- or

```html
<img
	src="https://images.unsplas h.com/photo-1554597998- 97267a7?bob?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entr opy&cs=srgb&ixid=eyJhcHBfawQiojEONT8sfQ"
	alt="Dog says 'I can make it to the fence in 2.8 seconds. Can you?'"
	width="400"
	height="400"
/>
```

---

_TabIndex_

- [DOC of TabIndex](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex)
- adds interactivity to the element
- value 0 doesn't put the element in order make that just 'tabbable'
- value -1 removes the element from 'tabbable' group but still focussable with js
- value 1 to x is the tab order of focussing elements

---

_Bypass Block (skip links)_

- are mechanisms that skip over repeated material on a webpage. They are important for users who navigate with a keyboard because as they will allow users to skip over repeated sections and go to the content they are looking for immediately.
- The example below is a simple example of how a skip navigation functions. The “skip to main content” link will skip over the list of websites and jump to the main content of the page instead. This allows for a keyboard user to bypass the section and get to the main information right away instead of having to go through every single link before getting to the main information.
- CSS is added to make the skip navigation link invisible until it gains keyboard focus. Once it gains focus it becomes visible to a sighted user and will also be read by a screen reader user. The CSS used sets the link at the top left corner of the page. Once tabbed into, it will gain focus and be visible to sighted users. It will also be read by screen readers.
- [DOC of Skip links](https://webaim.org/techniques/skipnav/)


```html
<body>
	<div id="skip-to-main">
		<a href="#main-content">Skip to main content</a>
	</div>
	<nav>
		<ol>
			<li><a href="https://www.google.com" target="_blank">Google</a></li>
			<li><a href="https://www.youtube.com" target="_blank">Youtube</a></li>
			<li><a href="https://www.webaim.org" target="_blank">WebAIM</a></li>
			<li><a href="https://www.csun.edu" target="_blank">CSUN</a></li>
		</ol>
	</nav>

	<main id="main-content">
		<h1>Welcome!</h1>
		<div tabindex="0">How is your day going?</div>
	</main>
</body>
```

```css
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
```

- or

```html
<style>
	  .skip a{
	  position:absolute;
	  left:10000px;
	  top:auto;
	  width:1px;
	  height:1px;
	  overflow:hidden;
	  font-size:2em;
	  background:white;
	  font-weight: bold;
	  display:block;
	  padding:10px;
	}

	.skip a:focus{
	  position:static;
	  width:auto;
	  height:auto;
	}
</style>

<div class="skip"><a href="#content">Skip to Main Content</a></div>
<main id="content">// main content</main>
```

---
