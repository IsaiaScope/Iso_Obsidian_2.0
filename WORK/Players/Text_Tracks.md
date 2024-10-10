# Text Tracks

| Type                                                                                                 | Happy? |
| ---------------------------------------------------------------------------------------------------- | ------ |
| [Web Video Text Tracks Format (WebVTT)](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API) | :)     |

## Filmbank Examples

Sottotitoli wvtt: per verificare se supportati controllare DDSTextDisplayer (Shaka-player API) e se i colori si vedono bene i colori sono una specie di sottitoli che usano HTML

il file _locals.js_ è un import di Shaka-player che è una lista enorme di latino e English per i sottotitoli, se una lingua non è una di queste due non si vedono bene i sottotitoli quindi in _+../../dds/language_mapping.js_ è possibile aggiungere alla lista label per gestire nuove lingue.

Configuring text displayer - wvtt perchè colori e html devono essere rispettati e aggiungere lingue in più

---
