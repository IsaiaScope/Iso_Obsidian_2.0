- [[accessibility-udemy-theory.pdf | PDF of General Lines to Follow]]
- [DOC of building an accessible menu systems](https://www.smashingmagazine.com/2017/11/building-accessible-menu-systems/)
- Checklist
	- [DOC of a checklist for good accessibility](https://www.a11yproject.com/checklist/)
---
_ACCESSIBILITY GUIDELINES_
- **WCAG**: Web Content Accessibility Guidelines
	 - [DOC of Official Website](https://www.w3.org/WAI/standards-guidelines/wcag/)
	- [DOC of How to code Accessibility](https://www.w3.org/WAI/WCAG21/quickref/?currentsidebar=%23col_overview#text-alternatives)
---

_HEADINGS_

- CSS can make a heading look like whatever you want
- Pages should have a single h1
- Can have multiple h2 tags
- Headings should be hierarchical
- Do not skip headings
- just one h1 on top the page, than again h2 and so on going down the page and inside sections of the page. In other words inside

---

_ZOOM_

- site must be scalable at least to 400%
- also elements on page must be responsive to zoom level

---

_TEXT LAYOUT_

- Use paragraph tags
- Consider line spacing
- Strong contrast
- _COLOR BLIND_
	- [DOC of understanding color blindness guide](https://cruxcollaborative.com/insights/understanding-color-blindness-guide-to-accessible-design)
- _CONTRAST RATIOS_
  - [Check With CONTRAST TOOLS](https://webaim.org/resources/contrastchecker/)
  - _COLOUR AS AN INDICATOR_
  - Not everyone can see colour
  - _Never use colour as the sole indicator of what is happening use alse imgs_
    - Examples
      - Success messages
      - Error messages
      - Buttons
      - Links
- _CONTRAST RATIOS CLASSIFICATION_
  <br>

  |    Text     | AA compliant | AAA compliant |
  | :---------: | :----------: | :-----------: |
  | Normal text |    4.5:1     |      7:1      |
  | Large text  |     3:1      |     4.5:1     |

---

_Anchor Tag (a)_

- When to use:
  - To send the user to another webpage.
- When not to use:
  - With "fake" URLs and onClick handlers.

_Buttons_

- [[button&checkbox]]

_NOTES for Anchors and Buttons_

- You can use CSS to style links and buttons however you wish
- Always use more than just colour to indicate something
- Underlines, for example Visited links
- Descriptive text over "click here"

---

_IMG_

- ALT ATTRIBUTES
  - The alt attribute provides a textual description for those who cannot see the image.
- Blank alt attributes should be used when the image is purely for display and does not form part of the content.

_TEXT AND IMAGES_

- Avoid using text inside images where possible
- Screen readers cannot read it
- Accessibility tools that modify the text style (i.e. contrast) cannot work with it
  - If you must use it, make sure it conforms
  - to colour contrast standards
  - 3:1 for AA compliance

---

_VIDEO PRODUCTION_

- Avoid flashing imagery
- Consider making audio descriptions while shooting the video
- When editing your video, if you must use text, make it sure it is large and has a high contrast ratio

_PLAYER_

- Use native tags where possible
- Do not autoplay
- Make sure the video controls are accessible

_TRANSCRIPTS_

- Always provide a transcript where possible
- Many able-bodied people prefer to read a transcript
- Browsing on public transport, for example
- Transcript services
- Fiverr, Rev
- Makes your content more searchable!

_CAPTION_

- Captions are a description of what is happening and what is being said
- This is different from subtitles, which typically refers to a foreign language transition And does not include descriptions
- Ways to add captions:
  - Tools like Subs Factory
  - Caption services like Rev
  - Upload to Youtube and edit

---
