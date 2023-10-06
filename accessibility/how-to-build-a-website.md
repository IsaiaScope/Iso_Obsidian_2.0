HEADINGS
CSS can make a heading look like
whatever you want
Pages should have a single <h1>
Can have multiple <h2> tags
Headings should be hierarchical
Do not skip headings
- just one H1 on top the page, than again h2 and so on going down the page and inside sections of the page. In other words inside 

TEXT LAYOUT
> Strong contrast (see colour lesson)
Make it bigger
> Use paragraph tags
> Consider line spacing
Font choices

Tag 
<a>
When to use
To send the user to another
webpage.
When not to use
With "fake" URLs and onClick
handlers.

<button>
When to use
To perform some functionality
such as submitting a form or
making on-page changes.
When not to use
A simple link to another webpage.

You can use CSS to style links and
buttons however you wish
Always use more than just colour to
indicate something
Underlines, for example
Visited links
Descriptive text over "click here"

CONTRAST RATIOS

| Text | AA compliant | AAA compliant |
|:-----:|:-------------:|:---------:|
| Normal text | 4.5:1 | 7:1 |
| Large text | 3:1 | 4.5:1 |




CONTRAST TOOLS https://webaim.org/resources/contrastchecker/

COLOUR AS AN INDICATOR
Not everyone can see colour
Never use colour as the sole indicator of
what is happening
Examples
Success messages
Error messages
Buttons
Links

img
ALT ATTRIBUTES
The alt attribute provides a textual description for those who cannot see the image.
Blank alt attributes should be used when the image is
purely for display and does not form part of the
content.
In this example, the icons provide no additional
meaning than the text, so we would be repeating
ourselves.

TEXT AND IMAGES
Avoid using text inside images where
possible
Screen readers cannot read it
Accessibility tools that modify the text
style (i.e. contrast) cannot work with it
> If you must use it, make sure it conforms
to colour contrast standards
3:1 for AA compliance

VIDEO PRODUCTION
Avoid flashing imagery
Consider making audio descriptions
while shooting the video
When editing your video, if you must use
text, make it sure it is large and has a
high contrast ratio

PLAYER
Use native tags where possible
cvideo>
Do not autoplay
Make sure the video controls are
accessible

TRANSCRIPTS
Always provide a transcript where
possible
Many able-bodied people prefer to read a
transcript
Browsing on public transport, for
example
Transcript services
Fiverr, Rev
Makes your content more searchable!

CAPTION
Captions are a description of what is
happening and what is being said
This is different from subtitles, which
typically refers to a foreign language
transition
And does not include descriptions
Ways to add captions
Tools like Subs Factory
Caption services like Rev
Upload to Youtube and edit

LABELS
Inputs should be paired with labels

PLACEHOLDERS
Placeholders are not a replacement for labels
They should act as an additional hint, rather than presenting the same information
as the label
For example, placeholder="Date of birth" should be avoided because this
information should alteady be placed in the label
Whereas placeholder-"DD/MM/YYYY" provides an additional hint
Or just avoid them altogether

DISABLING BUTTONS
Avoid using the disabled attribute on buttons
Adding the disabled attribute takes the button out of the tab index and therefore
essentially disappears from the page
If the form is not in a submit-able state, allow the user to submit anyway and tell
them what they need to change

HANDLING ERRORS
This is the most difficult part of making forms accessible
It must be clear to the user that the form is invalid and what needs to be changed
Ideally, take the user to the source of the error
You can use ariadescribedby to identify the error for that field
<label for="email"> Email address</label>
<input type="email" name="name" aria-invalid="true" aria-describedby="email-error-details" />
<p id="email-error-details">This email address is
already registered.</p>
---


