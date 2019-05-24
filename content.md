layout: true
  
<div class="my-header"></div>

<div class="my-footer">
  <table>
    <tr>
      <td style="text-align:right">Sächsische Landesbibliothek – Staats- und Universitätsbibliothek</td>
      <td>06.06.19</td>
      <td style="text-align:right"><a href="https://www.slub-dresden.de/">www.slub-dresden.de</a></td>
    </tr>
    <tr>
      <td style="text-align:right">Referat 2.5</td>
      <td />
    </tr>
  </table>
</div>

<div class="my-title-footer">
  <table>
    <tr>
      <td style="text-align:left"><b>Kay-Michael Würzner</b></td>
    </tr>
    <tr>
      <td style="text-align:left">Referat 2.5</td>
    </tr>
    <tr>
      <td style="font-size:8pt"><b>6. Juni 2019</b></td>
    </tr>
    <tr>
      <td style="font-size:8pt">IT-Kolloquium, SLUB Dresden</td>
    </tr>
  </table>
</div>

---

class: title-slide
count: false

# (Open-Source-)OCR-Workflows
## Eine Aufriss über Techniken und Werkzeuge zur automatischen Texterkennung

---

# Überblick

- Einleitung
  + Was ist OCR?
  + Wozu benutzt man OCR?
  + Warum überhaupt OCR?
- Technische Aspekte
  + Komponenten eines einfachen OCR-Workflows
  + Modelltraining
  + Optimierungsoptionen
  + Komplexere OCR-Workflows
- Nichttechnische Aspekte
  + OCR-D
  + Open-Source, und dann?

---

class: part-slide
count: false

# Was ist OCR?

---

# Was ist OCR?

.cols[
.sixty[
- **O**ptical **C**haracter **R**ecognition: Automatische Erfassung von Text in Bildern
- ursprünglich begrenzt auf Zeichenerkennung
- heute häufig Synonym für den gesamten Texterfassungsprozess
  + Bildvorverarbeitung
  + Layoutanalyse (OLR)
  + Zeilenerkennung
  + ...
]
.fourty[
<center><img src="img/times.svg" /></center>
]
]

---

# Zeichenorientierte Ansätze

.cols[
.seventy[
- Erkennung erfolgt *glyphenweise*
  - **Mustervergleich**: Vergleich der Zeichenbilder zu in einem „Setzkasten“ gespeicherten Glyphen **Pixel für Pixel**
  - **Merkmalsvergleich**: Zerlegung der Glyphen in vordefinierte, bedeutungstragende **Eigenschaften** wie *Einfärbung*, *Kurven*, *Linien* etc. und Vergleich zu Referenzmaterialien
- Kombination beider Ansätze!
- Zerlegung der Seite in *Zeilen* und *Zeichen* notwendig
- Vorgehen in `ABBYY FineReader`
]
.fourty[
<center><img src="img/char.svg" width="300px" /></center>
]
]

---

# Zeilenorientierte Ansätze

- Erkennung erfolgt *zeilenweise*
  1. **Skalierung:** einheitliche Höhe für alle Zeilen
  2. **Merkmalsextraktion**: Raster mit festgelegter Anzahl (horizontaler) Zeilen und variabler Anzahl (vertikaler) Spalten → Zeilen als Sequenzen binärwertiger Vektoren fixer Länge
<center><img src="img/grid.svg" width="800px"/></center>
- kontextsensitive Erkennung über *Übergangswahrscheinlichkeiten* der Vektoren
- Zerlegung der Seite in *Zeilen* notwendig
- Vorgehen *robuster* gegenüber Varianz durch Artefakte als zeichenorientierte Ansätze
- `Tesseract` (ab Version 4), `OCRopus`, `kraken`, `Calamari`
  + Einsatz *neuronaler Netze* für die Sequenzklassifikation

---

class: part-slide
count: false

# Wozu benutzt man OCR?

---

# Wozu benutzt man OCR?

.cols[
.sixty[
- typische Anwendungen
  - Nummernschilderkennung
]
.fourty[
<center>
<img src="img/anpr.svg" width="300px" />
<p style="font-size:4pt;">Image by Achim Raschka, CC BY-SA 3.0</p>
</center>
]
]

---

count: false

# Wozu benutzt man OCR?

.cols[
.sixty[
- typische Anwendungen
  - Nummernschilderkennung
  - Captcha-Umgehung
]
.fourty[
<center>
<img src="img/captcha.svg" width="300px" />
<p style="font-size:4pt;">Image by JD, CC BY-SA 2.0</p>
</center>
]
]

---

count: false

# Wozu benutzt man OCR?

.cols[
.sixty[
- typische Anwendungen
  - Nummernschilderkennung
  - Captcha-Umgehung
  - Schlüsselinformationsextraktion
]
.fourty[
<center>
<img src="img/deposit.svg" width="300px" />
<p style="font-size:4pt;">Image by Eluminary, CC BY-SA 2.0</p>
</center>
]
]

---

count: false

# Wozu benutzt man OCR?

.cols[
.sixty[
- typische Anwendungen
  - Nummernschilderkennung
  - Captcha-Umgehung
  - Schlüsselinformationsextraktion
  - Handschrifterkennung
]
.fourty[
<center>
<img src="img/transkribus.svg" width="300px" />
</center>
]
]

---

count: false

# Wozu benutzt man OCR?

.cols[
.sixty[
- typische Anwendungen
  - Nummernschilderkennung
  - Captcha-Umgehung
  - Schlüsselinformationsextraktion
  - Handschrifterkennung
  - Volltextdigitalisierung
]
.fourty[
<center>
<img src="img/beauvais_0.svg" width="300px" />
<p style="font-size:4pt;">Image by Uwe Springmann, CC BY-SA 4.0</p>
</center>
]
]

---

count: false

# Wozu benutzt man OCR?

.cols[
.sixty[
- typische Anwendungen
  - Nummernschilderkennung
  - Captcha-Umgehung
  - Schlüsselinformationsextraktion
  - Handschrifterkennung
  - Volltextdigitalisierung
]
.fourty[
<center>
<img src="img/beauvais_1.svg" width="300px" />
<p style="font-size:4pt;">Image by Uwe Springmann, CC BY-SA 4.0</p>
</center>
]
]

---

class: part-slide
count: false

# Warum überhaupt OCR?

---

# Warum überhaupt OCR?

- OCR ist immer **fehlerhaft**! Aber:
- verändertes „Rechercheverhalten“ in Zeiten zunehmender Verfügbarkeit digitaler Quellen
  + Wissenserwerb durch Internetsuche
  + Sekundärliteratur (fast) vollständig **textdigital** verfügbar
  + Navigationssystem vs. Autoatlas
- Ansprüche an Verfügbarkeit von Primärquellen wächst
- vielfältige quantitative Auswertungsmethoden (i.e. *distant reading*)
- für den **digitalen Geisteswissenschaftler**: Bruch mit dem „Diktat der Verfügbarkeit“

---

class: part-slide
count: false

# Komponenten eines einfachen OCR-Workflows

---

# Komponenten eines einfachen OCR-Workflows

.cols[
.fifty[
]
.fourty[
<center>
<img src="img/grenzboten_raw.svg" />
</center>
]
]

---

count: false

# Komponenten eines einfachen OCR-Workflows

.cols[
.fifty[
- Bildvorverarbeitung
]
.fourty[
<center>
<img src="img/grenzboten_raw.svg" />
</center>
]
]

---

count: false

# Komponenten eines einfachen OCR-Workflows

.cols[
.fifty[
- Bildvorverarbeitung
]
.fourty[
<center>
<img src="img/grenzboten_opt.svg" />
</center>
]
]

---

count: false

# Komponenten eines einfachen OCR-Workflows

.cols[
.fifty[
- Bildvorverarbeitung
- Layoutanalyse
]
.fourty[
<center>
<img src="img/grenzboten_opt.svg" />
</center>
]
]

---

count: false

# Komponenten eines einfachen OCR-Workflows

.cols[
.fifty[
- Bildvorverarbeitung
- Layoutanalyse
]
.fourty[
<center>
<img src="img/grenzboten_struct.svg" />
</center>
]
]

---

count: false

# Komponenten eines einfachen OCR-Workflows

.cols[
.fifty[
- Bildvorverarbeitung
- Layoutanalyse
- Texterkennung
]
.fourty[
<center>
<img src="img/grenzboten_struct.svg" />
</center>
]
]

---

count: false

# Komponenten eines einfachen OCR-Workflows

.cols[
.fifty[
- Bildvorverarbeitung
- Layoutanalyse
- Texterkennung
]
.fourty[
<center>
<img src="img/grenzboten_text.svg" />
</center>
]
]

---

# Komponenten eines einfachen OCR-Workflows: Bildvorverarbeitung

- Prozesse zur bestmöglichen Vorbereitung der Digitalisate für OLR und OCR
  + **Cropping**: Beschneidung des Digitalisats auf den Druckbereich
  + **Deskewing**: Rotation des Digitalisats zur Begradigung von Schrägstellungen
  + **Binarization**: Binäre Kodierung der Pixel (bedruckte Bereiche schwarz, nicht-bedruckte Bereiche weiß)
  + **Despeckling**: Entfernung von Bildartefakten (Verschmutzungen, sichtbare Papiermaserung etc.)
  + **Dewarping**: Begradigung von Wellen auf Zeilenebene
- starker Einfluss auf Erkennungsqualität
- besondere Relevanz für historische Vorlagen

---

# Komponenten eines einfachen OCR-Workflows: Cropping

.cols[
.fifty[
<center>
<img src="img/cropping_in.svg" height="300px" />
</center>
]
.fifty[
<center>
<img src="img/cropping_out.svg" height="300px" />
</center>
]
]

---

# Komponenten eines einfachen OCR-Workflows: Deskewing

.cols[
.fifty[
<center>
<img src="img/deskewing_in.svg" height="300px" />
</center>
]
.fifty[
<center>
<img src="img/deskewing_out.svg" height="300px" />
</center>
]
]

---

# Komponenten eines einfachen OCR-Workflows: Binarization

.cols[
.fifty[
<center>
<img src="img/binarization_in.svg" height="300px" />
</center>
]
.fifty[
<center>
<img src="img/binarization_out.svg" height="300px" />
</center>
]
]

---

# Komponenten eines einfachen OCR-Workflows: Despeckling

.cols[
.fifty[
<center>
<img src="img/despeckling_in.svg" height="300px" />
</center>
]
.fifty[
<center>
<img src="img/despeckling_out.svg" height="300px" />
</center>
]
]

---

# Komponenten eines einfachen OCR-Workflows: Dewarping

<center>
<img src="img/dewarping_in_out.svg" />
</center>

---

# Komponenten eines einfachen OCR-Workflows: Einfluss

.cols[
.fifty[
Zuletzt wird anders nichts darans/
Dir zacke1 dieser Erden r ’’
Die Sonne/Kindrr/Frenud’ vnd Hauß
Muß übergeben werden/ ’’
Denn dirNatnr erlässt vns’ mehr ’
Der streugenSchnld ondPflichr.
]
.fifty[
Zuletzt wird anders nichts darans/
Die Fackel dieser Erden
Die Sonne/Kinder/Frennnd’ vnd Hauß
Muß übergeben werden/
Denn deeNainr erlässt vns nicht
Der strengen Schuld vndPflicht.
]
]
