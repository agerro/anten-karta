# Öppna Sjökort

En mobilanpassad webbapp för sjökortsnavigering med GPS, stöd för flera kartlager och djupuppskattning.

**Live:** https://agerro.github.io/open-lakecharts/
**Repo:** https://github.com/agerro/open-lakecharts

---

## Innehåll

- [Komma igång](#komma-igång)
- [Gränssnittet](#gränssnittet)
- [Funktioner](#funktioner)
  - [GPS och positionering](#gps-och-positionering)
  - [Opacitetskontroll](#opacitetskontroll)
  - [Lagerhantering](#lagerhantering)
  - [Testposition](#testposition)
  - [Helskärmsläge](#helskärmsläge)
  - [Kalibrering](#kalibrering)
  - [Djupuppskattning](#djupuppskattning)
  - [Exportera och importera kalibrering](#exportera-och-importera-kalibrering)
- [Lägga till nya sjökort](#lägga-till-nya-sjökort)
- [Filstruktur](#filstruktur)

---

## Komma igång

Appen körs direkt i webbläsaren på mobil eller dator — ingen installation krävs.

### Installera som app på Android

1. Öppna https://agerro.github.io/open-lakecharts/ i **Chrome**
2. Tryck på ⋮ (menyknappen) → **Lägg till på startskärmen**
3. Appen installeras som en PWA och öppnas i helskärm utan webbläsarens adressfält

GPS kräver HTTPS, vilket GitHub Pages tillhandahåller automatiskt.

---

## Gränssnittet

```
┌─────────────────────────────────────────────────┐
│ ⚓ Öppna Sjökort v6.2      58° 1.234′ N         │
│ Anten sjökort · Mjörn      12° 29.456′ E        │
├─────────────────────────────────────────────────┤
│                                                 │
│                  KARTA                          │
│                                                 │
│  ┌─────────────────┐                            │
│  │ UPPSKATTAT DJUP │  ← djupbadge (vänster)     │
│  │ Medel · 10–20 m │                            │
│  └─────────────────┘                            │
│                                                 │
├─────────────────────────────────────────────────┤
│ ● Anten sjökort  [━━━━━━━━━━━━━━━━]  85%        │
│ [◎ Hitta mig] [⊞ OSM] [📍 Testa] [⛶] [☰]      │
└─────────────────────────────────────────────────┘
```

**Toppraden** visar appnamn, version och din aktuella GPS-position i grader och minuter.

**Opacitetsraden** (strax ovanför knapparna) styr transparensen på det sjökort du befinner dig över.

**Knapparna** längst ner ger snabb åtkomst till alla funktioner.

---

## Funktioner

### GPS och positionering

En grön pulsande markör visar din realtidsposition på kartan. En cirkel runt markören indikerar GPS-noggrannheten.

- **◎ Hitta mig** — centrerar kartan på din position. Om GPS inte startats ännu aktiveras det automatiskt.
- Koordinater visas löpande i toppraden (grader och decimalminuter, WGS84).
- GPS-pillen längst upp visar status: *Söker…*, *GPS aktiv · ±5m* eller felmeddelare.

### Opacitetskontroll

Raden direkt ovanför knapparna visar en slider för det sjökort du är "mest" över — baserat på kartans centrum eller din testposition.

- Dra slidern vänster för att tona ned sjökortet och se OSM-bakgrundskartan
- Dra höger för att dölja OSM och visa sjökortet tydligare
- Slidern byter automatiskt karta när du panorerar över till ett annat sjökort

Opaciteten sparas automatiskt och återställs vid nästa besök.

### Lagerhantering

Tryck **☰ Lager** för att öppna lagerpanelen.

Här kan du för varje sjökort:

| Kontroll | Funktion |
|---|---|
| Toggle (av/på) | Visa eller dölj sjökortet på kartan |
| Opacitetsslider | Finjustera transparensen |
| ⊕ Kalibrera | Öppna kalibreringsvyn för detta sjökort |
| ⛶ Passa | Zooma kartan till detta sjökorts utbredning |
| ↑ / ↓ | Ändra renderingsordning (vad som visas ovanpå) |

Längst ner i lagerpanelen finns knappar för att **exportera** och **importera** kalibrering (se [Exportera och importera kalibrering](#exportera-och-importera-kalibrering)).

### Testposition

Tryck **📍 Testa** för att simulera en position utan att fysiskt befinna dig där — praktiskt för att kontrollera kalibrering och djupdata vid ett specifikt ställe.

**Flöde:**

1. Tryck **📍 Testa** — knappen pulserar och texten byter till *"📍 Klicka…"*
2. Tryck varsomhelst på kartan — en guldmarkör placeras ut
3. Ett kort visas längst upp på skärmen med koordinaterna
4. Knappen byter till *"📍 Aktiv"* (guldfärgad)
5. Koordinatvisningen och djupuppskattningen utgår nu från testpositionen
6. Tryck **✕ Ta bort** på kortet för att återgå till riktig GPS

Tryck **📍 Testa** igen under pick-läget för att avbryta utan att sätta en position.

### Helskärmsläge

Tryck **⛶ Fullskärm** för att växla till helskärm. Tryck igen (eller *Esc*) för att gå ur.

> På iOS måste appen vara installerad som PWA för helskärmsläge.

### Kalibrering

Sjökorten laddas med standardpositioner baserade på koordinatgittret i kartbilden. Om GPS-markören inte stämmer med kartbilden behöver du kalibrera.

Öppna kalibrering via **☰ Lager → ⊕ Kalibrera** för det aktuella sjökortet.

#### Lägga till referenspunkter

Kalibreringen använder minst 2 referenspunkter och beräknar bästa passning med minsta-kvadrat-metoden. Fler punkter ger bättre precision.

**För varje referenspunkt:**

1. Tryck **+ Lägg till punkt** — panelen stängs och kartan visas
2. **Steg 1/2** — ett blått banner visas: *"Tryck på sjökortet vid en känd plats"*
   - Välj ett landmärke du kan identifiera på båda kartorna (brygga, udde, ö)
   - Tryck på platsen → ett kryss placeras ut
   - Tryck **✓ Bekräfta** (eller tryck på en annan plats för att flytta krysset)
3. **Steg 2/2** — sjökortet tonas ned, OSM-kartan visas: *"Tryck på samma plats i OpenStreetMap"*
   - Tryck på exakt samma geografiska plats i OSM
   - Tryck **✓ Bekräfta**
4. Punkten läggs till i listan — upprepa för fler punkter

När du lagt till 2+ punkter visas **medelfel i meter** och knappen **✓ Tillämpa** aktiveras.

> **Tips:** Välj punkter som är geografiskt långt ifrån varandra för bästa precision. Med 4+ punkter kompenseras även för skanningsskevhet.

#### Förinställningar

Under referenspunkterna finns en knapp för att **Återställa standard** — återgår till de initiala koordinaterna från `charts.json`.

#### Djupkalibrering

I samma panel, under referenspunkterna, finns en sektion för **Djupkalibrering**. Denna information används av djupuppskattningsfunktionen.

1. **Max djup** — skriv in sjöns/sjöns kända maximala djup i meter (t.ex. `30` för Anten)
2. **Grundfärg** — tryck *Sätt på karta*, sedan på ett känt grunt ställe i sjökortet (t.ex. en strandlinje). Färgen samplas automatiskt.
3. **Djupfärg** — tryck *Sätt på karta*, sedan på det mörkast blå området (djupaste punkten). Färgen samplas automatiskt.

Färgrutor visar de samplade RGB-värdena. Tryck *Ändra* för att justera.

Djupkalibreringen sparas i localStorage och kan exporteras till `calibration.json` för att synkroniseras med andra enheter.

### Djupuppskattning

När GPS-position (eller testposition) är aktiv och du befinner dig över ett sjökort visas ett djupkort nere till vänster:

```
┌────────────────────┐
│ UPPSKATTAT DJUP    │
│ Medel              │
│ 10–20 m            │
│ ⚠ Ungefärligt      │
└────────────────────┘
```

Uppskattningen baseras på pixelfärgen i sjökortsbilden vid din position:

- **Med djupkalibrering** — interpolerar med RGB-avstånd mellan dina kalibrerade grund- och djupfärger → ger ett ungefärligt djupvärde i meter
- **Utan kalibrering** — faller tillbaka på en generisk ljusstyrke-heuristik (ljusblå = grund, mörkblå = djupt)

> ⚠ Djupuppskattningen är aldrig exakt och ska aldrig användas för faktisk navigation. Den ger en grov fingervisning om var djupkurvorna befinner sig.

### Exportera och importera kalibrering

Kalibreringen sparas lokalt i webbläsarens localStorage och är enhetsbunden. För att använda samma kalibrering på flera enheter eller se till att den återställs vid ny installation finns export/import.

#### Exportera

1. Öppna **☰ Lager**
2. Tryck **⬇ Exportera kalibrering**
3. Filen `calibration.json` laddas ned

#### Importera

1. Öppna **☰ Lager**
2. Tryck **⬆ Importera**
3. Välj din sparade `calibration.json`

#### Permanent kalibrering via repot

Om `calibration.json` läggs i GitHub-repots rot laddas kalibreringen automatiskt av alla enheter som inte har en lokal override:

```
open-lakecharts/
├── calibration.json   ← läggs hit
├── charts.json
└── charts/
```

Prioritetsordning vid start:

1. **localStorage** (enhetens lokala kalibrering)
2. **calibration.json** i repot (om localStorage saknas för den kartan)
3. **defaultBounds** i `charts.json` (om ingen kalibrering finns)

---

## Lägga till nya sjökort

Nya sjökort läggs till genom att skicka en pull request till repot. Du behöver:

1. En skannad kartbild (JPG eller PNG rekommenderas, gärna 2000–3000 px bred)
2. Ungefärliga koordinater för kartans hörn (kan kalibreras i appen efteråt)

### Steg för steg

#### 1. Forka repot

Gå till https://github.com/agerro/open-lakecharts och tryck **Fork**.

#### 2. Lägg till kartbilden

Lägg bildfilen i mappen `charts/`:

```
charts/
├── anten.jpg        ← befintlig
└── din_karta.jpg    ← din nya fil
```

Namnge filen med gemener och bindestreck, t.ex. `mjorn-norra.jpg`.

#### 3. Uppdatera charts.json

Lägg till en ny rad i `charts.json`:

```json
{
  "version": "1.0",
  "charts": [
    {
      "id": "anten",
      "name": "Anten sjökort",
      "description": "1:50 000 · WGS84 · Lantmäteriet",
      "file": "charts/anten.jpg",
      "defaultBounds": {
        "sw": [57.950, 12.450],
        "ne": [58.030, 12.567]
      },
      "defaultOpacity": 0.85
    },
    {
      "id": "din-karta",
      "name": "Namn på din karta",
      "description": "Skala · Källa",
      "file": "charts/din_karta.jpg",
      "defaultBounds": {
        "sw": [LATITUD_SV, LONGITUD_SV],
        "ne": [LATITUD_NO, LONGITUD_NO]
      },
      "defaultOpacity": 0.85
    }
  ]
}
```

**Fälten:**

| Fält | Typ | Beskrivning |
|---|---|---|
| `id` | sträng | Unikt ID, används som nyckel i `calibration.json`. Ändra aldrig efter att det är satt. |
| `name` | sträng | Visningsnamn i appen |
| `description` | sträng | Kortfattad info (skala, källa) |
| `file` | sträng | Relativ sökväg till bildfilen |
| `defaultBounds.sw` | `[lat, lon]` | Sydvästra hörnet (WGS84, decimalgrader) |
| `defaultBounds.ne` | `[lat, lon]` | Nordöstra hörnet (WGS84, decimalgrader) |
| `defaultOpacity` | tal 0–1 | Standardtransparens |

> Behöver du hjälp med att hitta ungefärliga koordinater? Öppna [OpenStreetMap](https://www.openstreetmap.org), högerklicka på kartans sydvästra hörn och välj *Visa adress* — koordinaterna visas i adressfältet. Upprepa för nordöstra hörnet.

#### 4. Skicka en pull request

Committa dina ändringar och skicka en PR mot `main`. Beskriv vilken karta du lägger till och varifrån den kommer (källa och eventuell licens).

#### 5. Kalibrera i appen

Standardpositionerna är sällan helt exakta. När kartan är inlagd, kalibrera den i appen med referenspunkter (se [Kalibrering](#kalibrering)) och exportera `calibration.json` att inkludera i samma PR eller separat.

---

## Filstruktur

```
open-lakecharts/
├── index.html          Appen (HTML + CSS + JS, ~50 KB)
├── charts.json         Kartregister — lista över alla tillgängliga sjökort
├── calibration.json    Sparad kalibrering (genereras av appen, committas manuellt)
└── charts/
    ├── anten.jpg       Anten sjökort 2800×3620 px
    └── mjorn.jpg       Mjörn sjökort
```

### calibration.json — format

```json
{
  "version": "1.0",
  "exported": "2026-06-24T10:30:00.000Z",
  "note": "Genererad av Öppna Sjökort",
  "calibrations": {
    "anten": {
      "sw": [57.962, 12.443],
      "ne": [58.028, 12.571],
      "depthCal": {
        "maxDepth": 30,
        "shallowColor": { "r": 185, "g": 215, "b": 230 },
        "deepColor":    { "r":  55, "g":  85, "b": 140 }
      }
    }
  }
}
```

Filen laddas automatiskt av appen vid start för enheter som inte har en lokal kalibrering.
