best practice no external link, but if you need 
underline needed
https://medium.com/@svinkle/why-let-someone-know-when-a-link-opens-a-new-window-8699d20ed3b1
https://www.sitepoint.com/15-rules-making-accessible-links/

give more information by using aria-label or aria-describedby; the second solution is better because first screen reader is gonna read inside <a> tag (Stefany Web Design) and than aria-describedby="new_window" (opens in new window)
<a href="https://stefanywebdesign.info" rel="noopener" style="color: black;" 1 aria-label="Stefany Web Design opens in new window" target="_blank"> Stefany Web Design<svg></svg></a>

	 ‹div hidden id="new_window"> opens in new window </div>
	<a href="https://stefanywebdesign.info" rel="noopener" style="color: black;" aria-describedby="new_window" target="_ _blank"> Stefany Web Design<svg></svg></a>


img link
<a href="/" class="logo-holder"› ‹img class="logo" alt="Homepage" src=" /assets/template/images/logo/logo.svg" > </a>

