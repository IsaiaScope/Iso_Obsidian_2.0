---
tags:
  - players/Shaka_Player
---

# Theory

## Shaka Build Files Code Guide Lines

> quando i moduli non era supportato questo era lo standard per esportare ed importare funzionalità

goog.provide('shaka.cast.CastProxy'); - means _export_
goog.require('shaka.Player'); - means _import_
@privare - vuole dire veramente che è privato e _inaccessibile_

---
