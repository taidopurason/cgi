# CGI proovitöö
## Seadistamine
Kloonige repo:
```
git clone https://github.com/taidopurason/cgi
```
Liikuge projekti kausta (käsuga `cd cgi`) ning käivitage seal järgmised käsud:
```
npm install

npm run serve
```
Kui kõik õnnestub, on rakendus kättesaadav käsureal kuvatud aadressil (üldjuhul http://localhost:8080/).

## Lahenduse kirjeldus
Kasutasin arendamiseks **Vue.js** raamistikku ning kujunduse jaoks **Bootstrap**'i.

Kirjeldan lühidalt lahendust komponentide kaupa:
* **DayLength.vue** -
 Siin asub põhiline rakenduse struktuur ja loogika, sh ka koordinaatide ja kuupäeva järgi päeva pikkuse ning päikesetõusu ja
 -loojangu kellaaegade arvutamine. 
 Kõik kellaajad kuvatakse UTC ajatsoonis. 
 Kasutasin [*SunCalc*](https://github.com/mourner/suncalc)'i vajalike arvutuste tegemiseks.
    * **Form.vue** - koordinaatide ja kellaaja sisestamine
    * **Map.vue** - kaardi implementeerimine. Selleks kasutasin [*Leaflet*](https://leafletjs.com/) raamistikku.
Koordinaate saab valida kas soovitud asukoha peale vajutades või markerit liigutades, pärast mida uuendatakse leht automaatselt
uute andmetega.
    * **History.vue** - Graafiku kuvamine ja kuvatava ajavahemiku valimine
        * **ChartContainer.vue** ja **LineChart.js** - Graafiku joonistamine ja andmepunktide arvutamine. Kasutasin graafiku joonistamise jaoks
        [*Chart.js*](https://www.chartjs.org/) raamistikku. Arvutused tegin jällegi [*SunCalc*](https://github.com/mourner/suncalc)
        abil.               

Kokku kulutasin lahendamiseks hinnanguliselt 10 tundi.
Päris mitmete asjade jaoks jäi aega veidi puudu ning oleksin soovinud veel lisada:
* Sisendi valideerimise (sellega ka alustasin, kuid ei jõudnud valmis),
* Kellaaegade kuvamine ka vastavas ajavööndis,
* Automaatne asukoha tuvastus,

sest arvan, et need parandaksid kasutajakogemust.