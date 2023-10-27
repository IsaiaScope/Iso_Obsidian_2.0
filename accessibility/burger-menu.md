
https://codepen.io/Stefany93/post/good-hamburger-navigation

```html
<body>
	<!-- <button> is semantically correct for a hamburger because it is
  a button. Don't use <i>, <a> or <checkbox> -->
	<header class="main">
		<nav id="primary-nav" aria-label="Main">
			<div class="container">
				<a href="/" class="logo-holder">
					<img
						class="logo"
						src="http://c0162.paas1.syd.modxcloud.com/assets/template/images/logo/logo.svg"
						alt="Homepage"
					/>
				</a>
			</div>
			<button id="hamburger" aria-expanded="false">
				<span>Menu</span>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewbox="0 0 24 24"
					aria-hidden="true"
				>
					<path d="M0 0h24v24H0z" fill="none" />
					<path d="M3 18h18v-2H3v2zm0-5h18v-2H3v2zm0-7v2h18V6H3z" />
				</svg>
			</button>
			<div class="main-nav">
				<ul class="container">
					<li>
						<a href="#" aria-current="page">Home </a>
					</li>
					<li>
						<a href="#">New Patients</a>
					</li>
					<li>
						<a href="#">Testimonials</a>
					</li>
					<li>
						<a href="#">Hospital Gallery</a>
					</li>
					<li>
						<a href="#">Services</a>
					</li>
					<li>
						<a href="#">Appointments</a>
					</li>
					<li>
						<a href="#">Contact us</a>
					</li>
				</ul>
			</div>
		</nav>
	</header>
</body>
```

---

```js
document.addEventListener("DOMContentLoaded", function (event) {
	let hamburger = document.getElementById("hamburger");
	// If JS is enabled, it will un-expand the hamburger
	hamburger.setAttribute("aria-expanded", "false");
	hamburger.onclick = function () {
		if (this.getAttribute("aria-expanded") == "false") {
			this.setAttribute("aria-expanded", "true");
		} else {
			this.setAttribute("aria-expanded", "false");
		}
	};
});
```

---

```css
#hamburger {
	margin: 1em;
	justify-self: end;
	display: block;
	background: #fff;
	border: none;
	font-size: 1.2em;
	line-height: 1;
}
#hamburger #expanded {
	display: none;
}
#hamburger svg {
	width: 3em;
	height: 3em;
	background: white;
}
#hamburger span {
	display: block;
	position: relative;
	top: 15px;
}
#hamburger[aria-expanded='true'] {
	color: #000;
}
#hamburger[aria-expanded='true'] svg {
	background: #262262;
	fill: white;
}
#hamburger[aria-expanded='true'] span {
	position: static;
}
#hamburger[aria-expanded='true'] #expanded {
	display: block;
}
#hamburger[aria-expanded="true"] + .main-nav {
	display: block;
}
.js-enabled .main-nav {
	display: none;
}
@media (min-width: 848px) {
	.js-enabled .main-nav {
		display: block;
	}
}
```
