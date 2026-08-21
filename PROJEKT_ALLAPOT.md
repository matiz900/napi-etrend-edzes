# Projekt állapot — Napi Étrend-Edzés
Utolsó frissítés: 2026-08-21 (a GitHub repó `Fejlesztesi_naplo.html` alapján, a saját sablonunkba átalakítva)

## Állandó utasítások

- **Mi ez a projekt:** Egyfájlos, önálló PWA (HTML + vanilla JS, framework és
  build lépés nélkül) napi étrend- és edzéstervezéshez, kalóriaszámoláshoz.
- **Élő URL:** `matiz900.github.io/napi-etrend-edzes/`
- **Repo:** `github.com/matiz900/napi-etrend-edzes`
- **Helyi mappa (Windows):** `Documents\GitHub\napi-etrend-edzes`
- **Deployment workflow:** fájl szerkesztése → GitHub Desktop: Commit → Push.
  Nincs build lépés, nincs CI.
- **Tárolás:** böngésző `localStorage`, eszközönként — **nincs szerver, nincs
  szinkron eszközök között.**
- **Fő adatfájlok a repóban:**
  - `index.html` — a fő alkalmazás
  - `Fejlesztesi_naplo.html` — fejlesztési napló (eddig ez töltötte be a
    `PROJEKT_ALLAPOT.md` szerepét — innentől ezt a fájlt frissítjük helyette,
    de a régi is megmaradhat referenciának)
  - `Fogyas_alapelvek.html`, `Vitamin_es_gyulladascsokkentes.html` — a
    Tudástár fülbe beépített tartalmak forrásdokumentumai
  - `Napi_terv_etrend_edzes.html`
  - `manifest.json` + ikonok — PWA telepíthetőséghez
- **Kamera-engedély megjegyzés:** a vonalkód-beolvasás csak HTTPS-en (élesben,
  GitHub Pages-en) engedélyezett böngésző-oldalról, helyi fájlként megnyitva
  nem tesztelhető.
- **Fontos technikai buktató, amit ne ismételjünk meg:** a mentési réteg
  korábban `window.storage` hívásokat használt (fejlesztői/Claude-specifikus
  API), ami élesben, normál böngészőben nem működött volna — ez javítva lett
  valódi `localStorage`-ra épülő megoldásra. **Minden jövőbeli mentési logika
  kizárólag natív `localStorage`-t használjon**, sose Claude-specifikus vagy
  egyéb dev-only storage API-t.

## Hol tartunk most

Az app funkcionálisan gazdag és éles állapotban van, aktív, gyakori
fejlesztés alatt (két frissítés is történt 2026-08-18 és 2026-08-19-én).

**Elkészült funkciók:**
- Napi összefoglaló kártya kalóriagyűrűvel és makró-sávokkal
- Ételkönyvtár 5 kategóriában (143 tétel), makrókkal, hozzávalókkal
- Edzés adatbázis (terem + otthoni, A/B/C heti variánsok)
- TDEE-kalkulátor (Mifflin-St Jeor), automata víz- és fehérjeszükséglet
- Futás/kerékpározás MET-alapú kalória- és folyadékveszteség-számító
- Garmin Connect CSV import
- Étkezési idő push-értesítések, sötét mód, edzésnaplózó
- Napló/history fül (60 napos visszamenőleges nézet)
- Egyéni étel hozzáadása + JSON import/export
- PWA manifest + ikonok
- Vonalkód-beolvasás (BarcodeDetector API + Open Food Facts)
- Kávé makró kártya, napi be/kikapcsolóval
- Napi rost-tracker (25-35 g cél sávval)
- Alkoholmentes nap jelölő
- Tudástár fül (bővíthető cikk-adatbázis, jelenleg 2 cikkel)

## Mi készült el

### 2026-08-19 — Tudástár + rost/alkohol tracker
- Új "Tudástár" fül: bővíthető, kibontható kártyás tartalom, induláskor
  2 cikkel (gyulladáscsökkentés/bélflóra-helyreállítás, fogyás alapelvek)
- TDEE-kalkulátor kiegészítve automatikus napi víz- (testsúly × 0,033 l) és
  fehérjeszükséglet-kijelzéssel (testsúly × 1,8-2,2 g)
- Napi rost-tracker: manuális grammos hozzáadás, 30 g célhoz viszonyított sáv
- Alkoholmentes nap jelölő kapcsoló a Ma nézeten
- Két önálló referenciadokumentum a Tudástárhoz

### 2026-08-18 — Tárolás javítás, Challenge diéta, kávé, vonalkód
- **Kritikus javítás:** mentési réteg lecserélve valódi `localStorage`-ra
  (lásd fent, Állandó utasítások)
- Challenge nyári étrend (39 tétel) véglegesen beépítve a `FOOD_DB`-be
- Kávé makró kártya beállításokkal és napi be/kikapcsolóval
- Vonalkód-beolvasás: kamerás felismerés + kézi beviteli mező fallback,
  Open Food Facts lekérdezés

### Korábbi fejlesztések
- Barcode scanning előkészítés, Open Food Facts integráció tervezése
- PDF-to-JSON import workflow diéta PDF-ek onboardingjához
- Alapfunkciók: étel/edzés adatbázis, TDEE, MET-kalkulátor, Garmin import,
  PWA, sötét mód

## Döntések és indoklásuk

- **Egyfájlos, framework nélküli architektúra:** nincs build lépés, nincs
  keretrendszer — egyszerű szerkesztés + GitHub Desktop push a deploy.
  Ez tudatos döntés az egyszerűség és a gyors iterálás érdekében.
- **localStorage, nincs backend:** az adat eszköz-helyi marad, nincs
  szerveroldali tárolás vagy szinkronizáció — ez korlátozza a több-eszközös
  használatot, de nem igényel szervert, adatbázist vagy hitelesítést.

## Nyitott kérdések / ismert korlátok

1. **Nincs szinkron eszközök között** — tisztán eszköz-helyi `localStorage`.
2. **Vonalkód-felismerés Safari/iOS-en nem működik** — a `BarcodeDetector`
   API-t Chrome/Android natívan támogatja, Safari-n csak a kézi beviteli
   mező használható.
3. **Vonalkód-lekérdezés internetkapcsolatot igényel** — ez az egyetlen
   funkció, ami nem működik offline (minden más igen).
4. **Tudástár tartalma statikus, kódba ágyazott** — nincs admin felület,
   bővítés csak kód-szerkesztéssel lehetséges.

## Következő lépések — innen folytasd

1. Élesítés ellenőrzése GitHub Desktopon keresztül (Commit → Push), majd élő
   tesztelés — különös tekintettel a kamera-engedélyre.
2. Heti súlytrend grafikon a napló fülön, ~0,5 kg/hét cél vonallal összevetve.
3. Napi checklist widget (lépésszám cél, alvásidő cél) a fogyás alapelvek
   8 pontja alapján.
4. Tudástár bővítése további cikkekkel (pl. edzés utáni regeneráció, alvási
   higiénia).

## Kulcsreferenciák

- Élő app: `matiz900.github.io/napi-etrend-edzes/`
- Repo: `github.com/matiz900/napi-etrend-edzes`
- `Fejlesztesi_naplo.html` — a repóban lévő eredeti fejlesztési napló (ebből
  készült ez a `PROJEKT_ALLAPOT.md`)
- `GitHub_kezikonyv.html` — feltöltési/frissítési útmutató (ha létezik a
  helyi mappában — a repó fájllistájában nem szerepelt, [??] ellenőrizd)
- `Vitamin_es_gyulladascsokkentes.html`, `Fogyas_alapelvek.html` — Tudástár
  forrásdokumentumok

---
**Megjegyzés:** ez a fájl a repóban már meglévő `Fejlesztesi_naplo.html`
tartalma alapján készült, átalakítva a mi `PROJEKT_ALLAPOT.md` sablonunkba,
hogy a `/onboard` és `/handoff` skillek is tudják kezelni. Érdemes eldönteni,
hogy innentől melyik fájlt tartod karban elsődlegesként — javaslom, hogy a
`PROJEKT_ALLAPOT.md`-t, mert ez illeszkedik a többi projekted azonos nevű
állapotfájljához, és ezt frissíti automatikusan a `/handoff` skill.
