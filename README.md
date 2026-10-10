# Dagelijks Overzicht

Persoonlijk dagdashboard voor Chiang Rai en Nederland, in het warme Notion-ontwerp van de Pulse-sites. Eén statische pagina, zonder build-stap. Instellingen blijven bewaard onder `dashboard_config_v3` in `localStorage`, dezelfde sleutel als de vorige versie op `janvanhal.github.io`.

## Functies

- Kop met datum, twee klokken en knoppen voor verversen, thema, kolommen, tekstgrootte en instellingen.
- Wisselkoers EUR/THB met omrekenen van euro naar baht en terug. Eerst Frankfurter, daarna ExchangeRate-API. Een vaste koers kan ook.
- Weer voor twee plaatsen (standaard Chiang Rai en Noordwijk aan Zee): temperatuur, icoon, wind, neerslagkans, minimum en maximum, plus drie dagen vooruit. De radarlink volgt de plaats, tenzij je zelf een URL invult.
- Nederlands nieuws en sportnieuws van NOS, per rubriek, met tijd, titel en korte samenvatting. Een foto bij het eerste artikel kan aan of uit.
- Drie thema's: licht, medium en donker. Medium is warmer papier; donker gebruikt midnight met het cyaan van de Pulse-logo's.
- Lay-out: op een telefoon één kolom. Op een breed scherm twee kolommen, of drie met de widgets rechts.
- Tekstgrootte van artikelen in drie stappen. Foto's blijven even groot.
- Zichtbaarheid per klok, kaart, nieuwskolom en rubriek. Verborgen onderdelen laten geen leeg gat achter.

## Instellingen

Open het paneel met de tandwielknop. Niets verandert tot je op **Opslaan & Sluiten** drukt. **Annuleren**, **Sluiten**, Escape en een klik op de achtergrond gooien de wijzigingen weg. **Standaard** vraagt om bevestiging en zet daarna de beginwaarden terug.

Opgeslagen velden:

- Thema, 3-koloms weergave, foto bij het eerste artikel, tekstgrootte.
- Naam en IANA-tijdzone van beide klokken. Standaard `Asia/Bangkok` en `Europe/Amsterdam`.
- Naam, breedtegraad, lengtegraad en radar-URL van beide weerlocaties.
- Live koers aan/uit, en anders een vaste THB-koers per euro.
- Zichtbaarheid van klokken, kaarten, nieuwskolommen en rubrieken.

De laatste goede koers, het weer en het nieuws staan apart in `dailydash_cache_v1`. Nieuws wordt per rubriek maximaal eens per 15 minuten opgehaald, rubriek na rubriek. Verversen haalt alles opnieuw op en negeert die cache.

## Databronnen

- Wisselkoers: [Frankfurter](https://www.frankfurter.app/) (`api.frankfurter.app`), reserve [ExchangeRate-API](https://www.exchangerate-api.com/) (`open.er-api.com`).
- Weer: [Open-Meteo](https://open-meteo.com/).
- Radar: [RainViewer](https://www.rainviewer.com/) en [Buienradar](https://www.buienradar.nl/).
- Nieuws: NOS-RSS via [rss2json](https://rss2json.com/). Rubrieken: algemeen, politiek, wereld, economie, voetbal, wielrennen, schaatsen en sport algemeen.

## Bestanden

- `index.html` — pagina en scripts
- `styles.css` — Notion-tokens uit UnitPulse, plus de dashboardstijl
- `img/dailydash-icon.svg` — favicon en kop-icoon
