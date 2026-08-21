# Krankmeldung

Sprachauswahl-Seite für die Anleitungen zur Krankmeldung der Kläger Group.

**Live:** https://lisa-schneider1.github.io/Krankmeldung/

## Aufbau

| Datei              | Zweck                                                  |
|--------------------|--------------------------------------------------------|
| `index.html`       | Die komplette Seite (HTML + CSS in einer Datei)        |
| `km-<code>.webp`  | Infografik zum Anzeigen im Browser (wird verlinkt)     |
| `km-<code>.jpg`   | Dieselbe Grafik als JPEG zum Weiterleiten und Drucken  |
| `flag-<code>.webp` | Flaggen-Sticker für die Sprachliste, 144 × 120 px      |
| `logo.png`         | Kläger-Group-Logo, 900 × 127 px                        |

`<code>` ist der ISO-639-1-Sprachcode:

| Code | Sprache     | Code | Sprache    |
|------|-------------|------|------------|
| `de` | Deutsch     | `it` | Italiano   |
| `en` | English     | `ru` | Русский    |
| `tr` | Türkçe      | `ro` | Română     |
| `hr` | Hrvatski    |      |            |

## Regeln für neue Dateien

- **Nur ASCII in Dateinamen** – keine Umlaute, keine Leerzeichen. Umlaute in
  URLs müssen kodiert werden und führen auf manchen Servern zu 404-Fehlern.
- **Kleinbuchstaben**, Wörter mit Bindestrich getrennt.
- Jede Sprache braucht **drei** Dateien: `km-<code>.webp` (Anzeige),
  `km-<code>.jpg` (Weiterleiten/Drucken) und `flag-<code>.webp` (Sticker).
- Die Flaggen sind bewusst **Bilder statt Emoji**: Windows liefert keine
  Flaggen-Emoji mit, dort erschienen stattdessen die Buchstabenpaare „DE",
  „GB" usw.

## Neue Sprache hinzufügen

1. Grafik als WebP und JPEG unter `km-<code>.webp` / `km-<code>.jpg`
   hochladen (siehe „Bilder aufbereiten“).
2. In `index.html` im Block `<div class="language-list">` eine Zeile ergänzen:

   ```html
   <a class="language" href="km-pl.webp">
     <img class="flag" src="flag-pl.webp" alt="" width="48" height="40">
     <span class="language-name">Polski</span>
     <span class="arrow">›</span>
   </a>
   ```

3. Im Block `<div class="dl-list">` den JPEG-Link ergänzen:

   ```html
   <a href="km-pl.jpg" download>Polski</a>
   ```

## Bilder aufbereiten

Die Quellgrafiken sind ca. 1024 × 1536 px. Direkt exportierte PNGs sind rund
1,5 MB groß und für mobile Nutzung zu schwer. Mit Python und Pillow
(`pip install pillow`) auf ca. 200 KB bringen:

```python
from PIL import Image

im = Image.open("quelle.png").convert("RGB")
im.save("km-xx.webp", "WEBP", quality=90, method=6)
im.save("km-xx.jpg", "JPEG", quality=85, optimize=True,
        progressive=True, subsampling=0)
```

`subsampling=0` beim JPEG ist wichtig, damit farbiger Text scharf bleibt.

### Flaggen-Sticker

Die Sticker müssen einen **echten transparenten Hintergrund** haben. Nicht die
Vorschau abfotografieren oder screenshotten – dabei landet das graue
Schachbrettmuster als echte Pixel im Bild und ist später auf dem hellen Button
sichtbar. Immer die PNG-Datei selbst herunterladen.

Ausgabeformat: 144 × 120 px WebP mit Alphakanal, der Sticker (ohne die blauen
Funken-Striche) exakt 112 px breit und mittig platziert – so wirken alle
Flaggen in der Liste gleich groß. Angezeigt wird mit 48 × 40 px, die
dreifache Auflösung hält sie auf hochauflösenden Displays scharf.

## Lokal testen

```bash
python -m http.server 8765
```

Dann http://localhost:8765 im Browser öffnen.
