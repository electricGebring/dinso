# Teknisk uppgift – arbetslogg

## Mål

Vad ska förbättras och vilket problem löser lösningen?

- Problem:
- Mål:
- Viktigaste användaren:

## Utgångsläge

Vad upptäckte jag när jag läste den befintliga applikationen?

- Befintligt beteende:
- Relevanta komponenter, vyer eller routes:
- Behörighet och dataflöde:
- Viktiga antaganden:

## Avgränsning

### Ingår

- Ärendeöversikt
- Sökning och filtrering
- Ärendedetaljer
- Godkänna åtgärd
- Loading-, fel- och tomt läge
- Kundspecifika skillnader
- Responsivitet

### Ingår inte

- 

## Arbetslogg

Skriv kort efter varje större steg: vad du gjorde, vad du upptäckte och vilket beslut du tog.

| Datum | Vad gjorde jag? | Beslut eller lärdom |
|---|---|---|
| 24-09-2026  | Börjar kolla runt och ska dra igång appen. Försöker förstå vad jag tittar på:). Bygger olika kunder/användare  | ---- |
| 24-09-2026 | Tittar på filter och hur man kan sorter efter status och förallodatum| --- |
| 25-09-2026 | Sätter mig in i hur de olika rollerna sätts, och börjar på en mer granulär lösning | Snårigt! |
| 25-09-2026 11:12 | Skapade och testade de tre behörighetsfallen | Maja kan se och godkänna ärenden.<br>Linn kan se men inte godkänna ärenden.<br>Norah kan inte se ärenden.<br>Direkt URL till ärenden skyddas och skickar obehörig användare till företagsöversikten. |

## Viktiga beslut

### Beslut: 

- Alternativ jag övervägde:
- Vad jag valde:
- Varför:

## Verifiering

- [ ] Typecheck/build fungerar
- [ ] Ärenden kan sökas och filtreras
- [ ] Detaljvy fungerar
- [ ] Behörighet hanteras korrekt
- [ ] Lyckat resultat och fel visas
- [ ] Kundspecifika flöden fungerar
- [ ] Vyn fungerar på smal skärm

Kommandon och manuella tester:

```text

```

## AI-användning

Vad använde jag AI till, och vad kontrollerade eller ändrade jag själv?

- 

## Slutsats

### Lösning

Kort beskrivning av vad som implementerades.

### Arbetsprocess

Kort beskrivning av hur jag gick från analys till implementation och verifiering.

### Kvarvarande begränsningar

- 
