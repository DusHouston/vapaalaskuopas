# Vapaalaskuopas

Staattinen vertailu- ja arvostelusivusto. **Ei ulkoisia riippuvuuksia** – koko sivusto rakentuu yhdellä Node-skriptillä, joten `npm install` ei ole tarpeen eikä mikään voi hajota päivityksissä.

---

## Pikaohje: julkaisu Cloudflare Pagesiin

1. Lataa nämä tiedostot GitHub-repositorioosi
2. Cloudflare → **Compute (Workers & Pages)** → **Create** → välilehti **Pages** → **Connect to Git**
3. Valitse repo ja anna asetukset:

   | Asetus | Arvo |
   |---|---|
   | Framework preset | `None` |
   | Build command | `npm run build` |
   | Build output directory | `dist` |

4. **Save and Deploy**

Sivusto päivittyy jatkossa automaattisesti aina kun työnnät muutoksia GitHubiin.

> **Ennen julkaisua:** vaihda `sisalto/asetukset.json` -tiedostoon oma domainisi kenttään `url`. Sitä käytetään sitemapissa ja kanonisissa osoitteissa, ja väärä osoite haittaa hakukonenäkyvyyttä.

---

## Kansiorakenne

```
sisalto/
  asetukset.json          Sivuston nimi, osoite, kategoriat
  artikkelit/*.md         Artikkelit ja vertailut
  sivut/*.md              Staattiset sivut (tietoa, tietosuoja...)
teema/
  pohja.html              HTML-runko
  tyylit.css              Ulkoasu
public/                   Kuvat ym. – kopioidaan sellaisenaan
build.mjs                 Generaattori
dist/                     Valmis sivusto (syntyy automaattisesti)
```

---

## Uuden artikkelin kirjoittaminen

Luo tiedosto `sisalto/artikkelit/oma-artikkeli.md`. Tiedoston nimestä tulee osoite.

```markdown
---
otsikko: Parhaat nousukarvat 2026
kuvaus: Lyhyt kuvaus hakutuloksiin, noin 150 merkkiä.
kategoria: varusteet
paivays: 2026-08-15
paivitetty: 2026-08-15
avainsanat: [Vapaalasku, Vertailu]
nosto: false
luonnos: false
---

Tähän artikkelin teksti.
```

**Kentät**

| Kenttä | Selitys |
|---|---|
| `otsikko` | Sivun otsikko, näkyy myös Googlessa |
| `kuvaus` | Meta description, 120–160 merkkiä |
| `kategoria` | Yksi `asetukset.json`-tiedoston kategorian `slug`-arvoista |
| `paivays` | Julkaisupäivä muodossa VVVV-KK-PP |
| `paivitetty` | Viimeisin päivitys – päivitä tämä kun muokkaat |
| `avainsanat` | Luo aihesivun `/aihe/nimi/`. Alle 3 artikkelin aihesivut piilotetaan hakukoneilta |
| `nosto` | `true` nostaa artikkelin etusivun kärkeen |
| `luonnos` | `true` piilottaa artikkelin hakukoneilta ja näyttää varoituksen |

---

## Erikoislohkot

Tavallisen Markdownin lisäksi käytössä on affiliate-sivustolle rakennettuja lohkoja.

### Ostonappi

```
:::osto
nimi: Tuotteen nimi
kuvaus: Yhden lauseen perustelu
hinta: 349 €
kauppa: Kaupan nimi
linkki: https://kumppanuuslinkki.example.com
teksti: Katso hinta
:::
```

Linkkiin tulee automaattisesti `rel="sponsored nofollow noopener"` ja nappiin näkyvä **mainos**-merkintä. Näin sekä Googlen ohjeet että kuluttajansuojan vaatimus mainonnan tunnistettavuudesta täyttyvät.

### Tiivistelmä, plussat ja miinukset

```
:::tiivistelma
- Tärkein havainto
- Toinen havainto
:::

:::plussat
- Hyvä asia
:::

:::miinukset
- Huono asia
:::

:::huomio
**Vinkki.** Korostettu huomiolaatikko.
:::
```

### Vertailutaulukko

Tavallinen Markdown-taulukko. Se muuttuu puhelimessa automaattisesti korttinäkymäksi.

```
| Malli | Hinta | Paino |
|---|---|---|
| Malli A | 299 € | 1,2 kg |
```

---

## Paikallinen kehitys

```bash
npm run dev
```

Avaa `http://localhost:4321`. Sivusto rakentuu uudelleen automaattisesti kun tallennat tiedoston.

Pelkkä rakennus ilman palvelinta:

```bash
npm run build
```

---

## Mitä sivusto tekee valmiiksi

- Sitemap (`/sitemap.xml`) ja robots.txt
- RSS-syöte (`/rss.xml`)
- Kanoniset osoitteet ja Open Graph -tagit
- Schema.org-merkinnät (Article + BreadcrumbList)
- Murupolku joka sivulla
- Automaattinen sisällysluettelo yli kahden väliotsikon artikkeleihin
- Kumppanuuslinkki-ilmoitus jokaisen artikkelin alussa
- Aihesivut avainsanoista, ohuet aihesivut merkitään `noindex`
- Tumma ja vaalea teema järjestelmän asetuksen mukaan
- Mukautuva taulukko mobiilissa
- 404-sivu

---

## Ennen ensimmäistä julkaisua

- [ ] Vaihda `url` tiedostoon `sisalto/asetukset.json`
- [ ] Täytä `sisalto/sivut/tietosuoja.md` omilla tiedoilla
- [ ] Tarkista `sisalto/sivut/tietoa.md` ja yhteystiedot
- [ ] Lisää kumppanuuslinkit `:::osto`-lohkoihin kun olet päässyt verkostoihin
- [ ] Lisää domain Google Search Consoleen ja lähetä sitemap
- [ ] Tee sama Bing Webmaster Toolsissa

---

## Brändivärien vaihto

Avaa `teema/tyylit.css` ja muokkaa tiedoston alussa olevia muuttujia `--brandi`, `--brandi-vaalea` ja `--korostus`. Tummalle teemalle on omat arvonsa hieman alempana.
