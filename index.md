---
title: "Web - Ontbijtkoekclicker"
date: 2022-07-28T20:56:17+02:00
draft: false
toc: true
headercolor: "teal-background"
taal: JavaScript, HTML & CSS
---

We gaan onze eigen versie van het spel Cookie Clicker maken met JavaScript en HTML!

<!--more-->

<!-- 
Huidige stijl:
- Opsomming met puntjes, tenzij:
	+ nadruk op het aantal items (bijv. "Het spel heeft _vier_ onderdelen:");
	+ we later in de tekst naar het getal willen refereren.
-->

## Introductie

In deze instructie gaan we voornamelijk aan de slag met JavaScript. 
Het doel van de instructie is het maken van een spel dat je in je browser kan spelen. 
Het spel is gebaseerd op het bekende spel 
[Cookie Clicker](https://orteil.dashnet.org/cookieclicker/).
Naast het coderen van de functionaliteit, kan je aan verschillende onderdelen van het spel een 
eigen draai geven!



## Benodigdheden

[comment]: # (Is er een betere manier om naar interne instructies te verwijzen?)

### Voorkennis
Voor het maken van de Ontbijtkoekclicker is het belangrijk om bekend te zijn met de volgende talen:
- HTML
- CSS
- JavaScript

Ben je hier nog niet mee bekend? Ga dan eerst aan de slag met de instructie [Web - Development](/instructies/web-development/).

Voor deze instructie gebruiken we dezelfde editor als bij de 
[Web - Development](/instructies/web-development/) instructie:
[Visual Studio Code](https://code.visualstudio.com/download). 

### Voorbereidde bestanden
Om meer tijd te kunnen besteden aan JavaScript en het personaliseren van het spel, 
beginnen we met een vooropgezette pagina.  
Deze bestaat uit de volgende bestanden: 
- [HTML](start/pagina.html)
- [CSS](start/style.css)
- [JavaScript](start/script.js).

Deze zijn te downloaden via: [Download bestanden](start.zip).
Zorg voordat je begint met de instructie dat je de pagina kan weergeven met je editor/plugin(s). 

{{< voorbeeld kop="Extra: afbeeldingen lokaal opslaan" >}}
De afbeeldingen die we gebruiken als voorbeeld maken geen deel uit van de download. 
Dit zorgt dat je af en toe lange URLs in de code voorbij ziet komen. 
Deze URLs kan je eventueel vervangen door de afbeeldingen te downloaden. 
Zet gedownloade afbeeldingen in dezelfde directory als waar [pagina.html](start/pagina.html) zich bevindt.   
Deze kan je vervolgens als volgt gebruiken:

{{< highlight html >}}
<img src="VOORBEELDNAAM.jpg">
{{< /highlight >}}
{{< /voorbeeld >}}

## Overzicht van het spel

We beginnen met een samenvatting van het spel, zodat het duidelijk is waar we naartoe werken. 
De leukste manier om hier een idee over te krijgen is natuurlijk door het origineel te spelen.

Het doel van het spel is om cookies te genereren. 
Dit kun je doen door op het koekje te klikken, maar je kunt niet eeuwig blijven klikken. 
Uiteindelijk wil je dat er zoveel mogelijk koekjes automatisch worden gegenereerd. 
Om het spel automatisch koekjes te laten genereren kun je Clickers kopen, 
deze klikken automatisch op het koekje. 
Je koopt Clickers door met koekjes te betalen. Bij iedere aankoop worden de Clickers duurder. 
Daarnaast kun je Powerups kopen met koekjes.
Deze Powerups verbeteren je Clickers zodat zij bijvoorbeeld sneller cookies genereren.

Het spel bestaat uit pakweg vier verschillende onderdelen:
 1. Een koekje waar je als speler op kan klikken. 
    Hier kan je ook je aantal koekjes (score) en het aantal koekjes dat je per seconde verdient zien.
 2. Een overzicht waarin je alle actieve Clickers kan zien.
 3. Een winkel waar je Clickers kan kopen. 
 4. Een winkel waar je Powerups kan kopen. 

## Relatie met de HTML-code (1)

In deze instructie focussen we voornamelijk op het schrijven van JavaScript-code.
Maar als je Cookie Clicker speelt, kom je niet enkel in aanraking met JavaScript.
Het spel gebruikt HTML-code om een "skelet" van de pagina weer te geven. 
Het spel kleedt het skelet vervolgens aan met behulp van CSS. 
Met JavaScript maken we de pagina uiteindelijk interactief, zodat je er dus een spel op kan spelen.

Om de drie talen op een georganiseerde manier samen te laten werken, 
hebben we een aantal keuzes in de opzet van het HTML-bestand gemaakt.
Die keuzes lichten we in dit deel toe, 
zodat je de individuele onderdelen op de HTML-pagina straks gemakkelijk 
kan beïnvloeden via JavaScript-code.

We beginnen met het HTMl bestand. Het bestand [pagina.html](start/pagina.html) bevat een `section` tag 
met daarin vier verschillende `div` tags:

{{< highlight html>}}
<section>
	<!-- In deze div kan de speler op de Cookie klikken. --> 
	<div id="klikruimte"></div>
	<!-- In deze div laten we van elk type Clicker zien hoeveel we er hebben. --> 
	<div class="tierdisplay rows" id="actieveClickers"></div>
	<!-- In deze div kan de speler de verschillende Clickers kopen. --> 
	<div class="tierdisplay rows" id="clickerWinkel"></div>
	<!-- In deze div kan de speler Powerups voor de Clickers kopen. --> 
	<div class="tierdisplay table" id="powerupWinkel"></div>
</section>
{{< /highlight >}}

Deze `div`s komen overeen met de onderdelen van het originele spel
en bepalen dus op welke plek van de pagina de onderdelen komen te staan.
Het belangrijkste onderdeel van de `div`s zijn de `id`s. 
Met behulp van de `id`s kunnen we deze `div`s bereiken en gebruiken in JavaScript.

Stel bijvoorbeeld dat we in het overzicht met actieve Clickers een Clicker toe willen voegen. 
Om te zorgen dat we vanuit de `div` met actieve Clickers opereren, bereiken we deze als volgt:

{{< highlight javascript>}}
const actieveClickerDiv = document.getElementById("actieveClickers")
{{< /highlight >}}

<!-- 
Een vergelijking met een bekende boomstructuur, bijvoorbeeld een filesystem, 
zou hier nog handig kunnen zijn.
-->

Kortom, elk onderdeel van het spel komt overeen met een `div` in de HTML-code!
Als we bijvoorbeeld in JavaScript iets aan de Powerups willen veranderen, 
zoeken we de Powerup `div` eerst op met de correcte `id`!
De `id` is in dit voorbeeld `powerupWinkel`.

## Relatie met de HTML-code (2)

We weten nu waar de onderdelen van het spel komen te staan, 
maar hoe zien die onderdelen er precies uit?
En wat staat er in de bijbehorende `div`s?

De meeste onderdelen bestaan uit een lijst, 
met een onderdeel (of: [element](https://nl.wikipedia.org/wiki/Lijst_(informatica))) voor elke rang (Engels: "tier") Clicker. 
Als voorbeeld kunnen we naar de Powerups kijken:

{{< highlight html>}}
<!-- In deze div kan de speler Powerups voor de Clickers kopen. --> 
<div class="tierdisplay table" id="powerupWinkel">
	<h3>Powerups</h3>

	<!-- Via deze div kan je de Deegroller Powerup kopen. Deze kan je als voorbeeld gebruiken. --> 
	<div class="tier0" onclick="koopPowerup(0)">
		<img class="icon" src="https://gartic.com.br/imgs/mural/__/__fera__/rolling-pin.png">
		<text>10$</text>
	</div>

	<div class="tier1" onClick="koopPowerup(1)">
	<img class="icon" src="???">
	<text>???$</text>
	</div>

	<div class="tier2" onClick="koopPowerup(2)">...</div>
	<div class="tier3" onClick="koopPowerup(3)">...</div>
	<div class="tier4" onClick="koopPowerup(4)">...</div>
</div>
{{< /highlight >}}

De Deegroller Powerup (tier 0) is hier volledig uitgewerkt. 
Het belangrijkste onderdeel van dit voorbeeld is de `class` van de elementen in de lijst. 
De `class` werkt op een soortgelijke manier als de `id`, maar een class is niet uniek. 
Hierdoor kunnen we het `class` systeem voor alle drie de lijsten gebruiken die we gaan maken.
Als iets met de goedkoopste Clicker te maken heeft, heeft het `class="tier0"`. 
De een na goedkoopste Clicker heeft `class="tier1"`, etc.

Als je een element zoekt, moet je nu echter wel zorgen dat je in de juiste lijst zoekt. 
De volgende code verkrijgt resultaten met `class="tier0"` uit 
<span style="color:red">alle lijsten</span> (actieve Clickers, Clickers winkel, Powerup winkel).

{{< highlight html>}}
const tierDiv = document.getElementsByClassName("tier0")
{{< /highlight >}}

Dat is niet de bedoeling als we bijvoorbeeld iets aan de tier 0 Powerup willen veranderen. 
In plaats daarvan zorgen we dat we eerst naar de Powerups kijken, 
en vervolgens naar die van tier 0:

{{< highlight html>}}
const actieveClickerDiv = document.getElementById("powerup")
const tierDiv = actieveClickerDiv.getElementsByClassName("tier0")[0]
{{< /highlight >}}

Let op: omdat `getElementsByClassName` meerdere resultaten _kan_ geven 
(in dit voorbeeld niet), kiezen we de eerste via `[0]`.

## Een klikbaar koekje

Aan het begin van het spel heb je nog geen Clickers en moet je handmatig je koekjes verdienen.
Dit doe je door op de grote afbeelding van het koekje te klikken. 
Hier komen enkele dingen bij kijken:
- elke klik op de afbeelding word geregistreerd via een `onClick="onClickCookie()"` HTML-attribuut;
- we houden de hoeveelheid koekjes bij in een variabele `geld` in JavaScript, deze begint op `0`;
- de functie `onClickCookie` verhoogt `geld` met `1`;
- een functie `updateGeld` zorgt dat de nieuwe waarde van `geld` in de HTML-code update;
- `onClickCookie` roept `updateGeld` aan na het verhogen van `geld`.

Om dit te implementeren, moet je goed letten op de `id`s van de `div`- en `h2` tags:

{{< highlight html>}}
<div id="klikRuimte">
	<img src="https://www.pngall.com/wp-content/uploads/2016/07/Cookie-PNG.png" onClick="onClickCookie()">
	<br>
	<!-- In deze h2 tags laten we de huidige hoeveelheid geld zien, 
		en hoeveel geld we per seconde verdienen. --> 
		<h2 id="geld">Geld: 0$</h2>
		<h2 id="geldPerSeconde">Geld per seconde: 0$</h2>
</div>
{{< /highlight >}}

Voor `updateGeld` heb je de `h2` die het geld weergeeft nodig. 
Als je deze eenmaal gevonden hebt, kan je de inhoud van de `h2` via `innerText` als volgt updaten:

{{< highlight javascript>}}
geldH2.innerText = "Geld: " + hoeveelheid + "$"
{{< /highlight >}}

{{< voorbeeld kop="Voorbeeldcode klikbaar koekje" >}}
{{< highlight javascript>}}
let geld = 0

/* Voegt 1$ toe en update de pagina */
function onClickCookie() {
    geld = geld + 1

    updateGeld(geld) /* Update de pagina! */
}

/* Verander het geld bedrag dat op de pagina word weergegeven naar hoeveelheid. */
function updateGeld(hoeveelheid) {
    const geldH2 = document.getElementById("geld")

    geldH2.innerText = "Geld: " + hoeveelheid + "$"
}
{{< /highlight >}}

Eventueel kan je `updateGeld` ook direct gebruik laten maken van de globale variabele `geld`.
{{< /voorbeeld >}}


## Clickers winkel

Als we eenmaal wat koekjes hebben, willen we het spel natuurlijk voor ons laten werken!
Hiervoor gebruiken we Clickers, die je kan kopen bij de Clickers winkel. 
In [pagina.html](start/pagina.html) zien we een `div` met `id=clickerWinkel`; 
dit is waar we de Clickers te koop zullen zetten.
In deze `div` vinden we de volgende HTML-code:

{{< highlight html>}}
<div class="tier1 ruimte" onClick="koopClicker(1)">
	<img class="icon" src="???">
	<text>???</text>
	<text>??? $</text>
</div> 
{{< /highlight >}}

Hier kan je je eigen Clickers in zetten, met een plaatje en een prijs!
Dit doe je door de `???` te vervangen. 
Als voorbeeld hebben we dit al gedaan voor `tier0` in het HTML-bestand; 
hier is een chefkok met zijn cloche te zien.
Je hoeft ze natuurlijk niet per se alle vijf toe te voegen.

Als je een paar Clickers hebt, kunnen we beginnen met het toevoegen van de functionaliteit. 
Laten we een paar doelen vaststellen:
- elke klik op een Clicker `div` word geregistreerd via een `onClick="koopClicker(n)"` HTML-attribuut;
- we maken een constante variabele met het maximum aantal Clickers van een tier, 
  om te zorgen dat ze later op de pagina passen [^1];
- we houden per tier Clicker bij hoeveel ze kosten;
- de functie `koopClicker` checkt of je genoeg geld hebt en of er nog niet teveel Clickers zijn;
- als dit het geval is, registreer dan de nieuwe Clicker en haal het geld weg (aankoop voltooid);
- update naderhand de pagina op basis van de veranderingen.

[^1]: Er is immers geen ruimte op het scherm voor 100 kok icoontjes!

Vooralsnog verdienen de Clickers dus nog geen koekjes voor ons, 
we houden enkel bij hoeveel we er van welke tier gekocht hebben. 

Tip: omdat we het onderdeel Actieve Clickers nog niet hebben, is het lastig te zien of de winkel werkt.
Het kan handig zijn om in `clickerWinkel` `console.log(aantalClickers[tier])` te gebruiken, 
om te zien hoeveel je er hebt na aanschaf.


{{< voorbeeld kop="Voorbeeldcode Clicker winkel" >}}
{{< highlight javascript>}}
const aantalClickers = [0, 0, 0, 0, 0]
const clickerKosten = [1, 10, 50, 100, 500]

let geld = 0
let maxAantalClickers = 14 /* Voor de basisfunctie ligt dit aan de grootte van je scherm. */

/* Koopt een Clicker, mits hier geld voor is en er nog ruimte is. Update de pagina. */
function koopClicker(tier) {
    if (aantalClickers[tier] >= maxAantalClickers) {
        return
    }
    if (geld < clickerKosten[tier]) {
        return
    }

    geld = geld - clickerKosten[tier]
    aantalClickers[tier] = aantalClickers[tier] + 1

    /* Update de pagina! */
    updateGeld(geld)
}


{{< /highlight >}}
{{< /voorbeeld >}}

## Actieve Clickers inkomsten toevoegen

In deze sectie zorgen we dat de actieve Clickers geld genereren. 
Dit doen we door elke seconde het inkomen van alle actieve Clickers bij elkaar op te tellen. 
Het totaal voegen we toe aan de hoeveelheid koekjes die we al hebben. 
Een voorbeeld:

| Clicker &nbsp;&nbsp;&nbsp;&nbsp; | Inkomen per seconde &nbsp;&nbsp;&nbsp;&nbsp; | Aantal &nbsp;&nbsp;&nbsp;&nbsp; | Totaal &nbsp;&nbsp;&nbsp;&nbsp; |
|:---|:---:|:---|:---|
| Kok | 1 | 4 | 4 |
| Clicker tier 1 | 2 | 1 | 2 |
| Clicker tier 2 | 4 | 3 | 12 |
| Clicker tier 3 | 0 | 0 | 0 |
| Clicker tier 4 | 0 | 0 | 0 | 
| | | __Som:__ | __18__ |

We hebben drie nieuwe functies nodig:
- `berekenGeldPerSeconde` voert de berekening uit het voorbeeld uit;
- `updateGeldPerSeconde` update de `div` met `id="geldPerSeconde"`;
- `koopClicker` (en later `koopPowerup`) voeren `updateGeldPerSeconde` uit bij aankoop van een nieuwe Clicker.
- `updateClickerGeld` voegt de inkomsten van de Clickers toe aan het totaal, 
  met behulp van `berekenGeldPerSeconde`.

Om de functie `updateClickerGeld` elke seconde uit te voeren kan je de volgende code gebruiken:

{{< highlight javascript >}}
/* Zorgt dat updateClickerGeld elke seconde activeert. */
setInterval(updateClickerGeld, 1000)
{{< /highlight >}}

{{< voorbeeld kop="Voorbeeldcode actieve Clickers inkomsten toevoegen" >}}
{{< highlight javascript >}}
const aantalClickerTiers = 5

const clickerInkomsten = [1, 2, 4, 8, 32]
const aantalClickers = [0, 0, 0, 0, 0]

function berekenGeldPerSeconde() {
    let totaal = 0

    for (let tier = 0; tier < aantalClickerTiers; tier++) {
        const inkomsten = clickerInkomsten[tier]
        const aantal = aantalClickers[tier]
        totaal += inkomsten * aantal
    }

    return totaal
}

/* Voegt het geld van de Clickers toe aan het geld. Update de pagina. */
function updateClickerGeld() {
    geld = geld + berekenGeldPerSeconde()

    /* Update de pagina! */
    updateGeld(geld)
}

/* Zorgt dat updateClickerGeld elke seconde activeert. */
setInterval(updateClickerGeld, 1000)

/* Verander het geld per seconde bedrag dat op de pagina word weergegeven naar hoeveelheid. */
function updateGeldPerSeconde(hoeveelheid) {
    const geldPerSecondeH2 = document.getElementById("geldPerSeconde")

    geldPerSecondeH2.innerText = "Geld per seconde: " + hoeveelheid + "$"
}
{{< /highlight >}}

Vergeet niet dat `koopClicker` (en later `koopPowerup`) `updateGeldPerSeconde` 
uitvoeren bij aankoop van een nieuwe Clicker!
{{< /voorbeeld >}}

## Actieve Clickers weergeven

We kunnen eindelijk werkende Clickers toevoegen, maar de pagina voelt nog erg leeg. 
Het zou leuk zijn om te zien welke Clickers we in dienst hebben!
Hiervoor gebruiken we de `div` met `id="actieveClickers"`.
Dit is hoe de `div` er uit zou moeten zien na het kopen van twee Chefs:

{{< highlight html >}}
<div class="tierdisplay rows" id="actieveClickers">
<h3>Actieve Clickers</h3>

<div class="tier0">
<!-- Per clicker van dit type voegen we een afbeelding toe. Er zijn nu 2 Chefs. --> 
<img src="https://pngimg.com/uploads/chef/chef_PNG54.png" class="icon">
<img src="https://pngimg.com/uploads/chef/chef_PNG54.png" class="icon">
</div>

<div class="tier1"></div>
...
</div>
{{< /highlight >}}

We gebruiken hier weer hetzelfde systeem als voorheen, waarin elk onderdeel van het spel een `div` is, 
welke een lijst met `class="tier"` elementen bevat. 
Die elementen kunnen we dus vinden zoals voorheen!
De vraag is nu: hoe voegen we hier afbeeldingen aan toe zoals in het bovenstaande voorbeeld?

Het antwoord: via `document.createElement`.
Via deze functie kunnen we in de JavaScript-code HTML-elementen maken, 
en deze later in een ander HTML-element toevoegen.
Dit is te zien in het volgende voorbeeld:

{{< highlight javascript >}}
const actieveClickerDiv = document.getElementById("actieveClickers")
const tierDiv = actieveClickerDiv.getElementsByClassName("tier0")[0]

const afbeelding = document.createElement("img")
afbeelding.src = "" /* jouw afbeelding bron */
afbeelding.className = "icon"

tierDiv.appendChild(afbeelding)
{{< /highlight >}}

Het voorbeeld laat alleen niet zien hoe je dit voor een willekeurige `tier` doet. 
Als je dit lastig vindt, vraag dan gerust om hulp!

{{< voorbeeld kop="Voorbeeldcode actieve Clickers" >}}
{{< highlight javascript >}}
/* Een lijst met de icoontjes van de Clickers. */
clickerIcons = [
    "https://pngimg.com/uploads/chef/chef_PNG54.png",
    /* tier 1 */
    /* tier 2 */
    /* tier 3 */
    /* tier 4 */
]

/* Voeg een nieuw icoontje toe aan de lijst met Actieve Clickers van tier tier */
function voegActieveClickerToe(tier) {
    const actieveClickerDiv = document.getElementById("actieveClickers")
    const tierDiv = actieveClickerDiv.getElementsByClassName("tier" + tier)[0]

    const afbeelding = document.createElement("img")
    afbeelding.src = clickerIcons[tier]
    afbeelding.className = "icon"

    tierDiv.appendChild(afbeelding)
}
{{< /highlight >}}

Van belang: gebruik `voegActieveClickerToe(tier)` aan het einde van de functie `koopClicker`, 
zodat het kopen van een Clicker van tier `tier` een nieuwe aan de lijst voor `tier` Clickers toevoegt.
{{< /voorbeeld >}}

## Clicker Powerup winkel

We kunnen nu handmatig koekjes verdienen en deze investeren in Clickers. 
De Clickers zijn te zien in het overzicht met actieve Clickers.
Het spel voelt echter een beetje eenzijdig: 
klikken > kopen > klikken > ... > einde(?).

Een manier om extra strategie toe te voegen is het introduceren van Powerups. 
Een Powerup zorgt dat de actieve Clickers van een bepaalde tier beter worden. 
In het voorbeeld kan je voor de Chef een deegroller kopen: 
deze zorgt dat de Chef twee keer zoveel koekjes per seconde kan produceren.

In deze instructie zullen we één Powerup per tier Clicker maken. 
Dit is net iets simpeler dan in het originele spel, 
maar je kan het later uitbreiden!
De HTML-code voor Powerups lijkt erg op die van de Clicker winkel:

{{< highlight html >}}
<!-- In deze div kan de speler Powerups voor de Clickers kopen. --> 
<div class="tierdisplay table" id="powerupWinkel">
<h3>Powerups</h3>
	<!-- Via deze div kan je de Deegroller Powerup kopen. Deze kan je als voorbeeld gebruiken. --> 
	<div class="tier0" onclick="koopPowerup(0)">
		<img class="icon" src="https://gartic.com.br/imgs/mural/__/__fera__/rolling-pin.png">
		<text>10$</text>
	</div>

    <div class="tier1" onClick="koopPowerup(1)">
		<img class="icon" src="???">
		<text>???$</text>
	</div>

	...
</div>
{{< /highlight >}}

Ook hier komt een kort plan van aanpak van pas:
- elke klik op een Clicker `div` word geregistreerd via een `onClick="koopPowerup(n)"` HTML-attribuut;
- we houden per tier Clicker bij of de Powerup al gekocht is;
- de functie `koopPowerup` check of je genoeg geld hebt en of de Powerup nog niet gekocht is;
- als dit het geval is, registreer dan de nieuwe Powerup en haal het geld weg (aankoop voltooid);
- update naderhand de pagina op basis van de veranderingen.

Ook moeten we zorgen dat we ergens een lijstje bijhouden met de inkomsten per seconde 
van elke Clicker. Als de Powerup is gekocht, vermenigvuldigen we die inkomsten met twee.

Tip: als de aanschaf van een Powerup succesvol is, 
moeten we zorgen dat men deze niet nogmaals kan kopen.
Je kan de Powerup uit de lijst halen via:

{{< highlight javascript >}}
const tierDiv = ...
tierDiv.remove()
{{< /highlight >}}

{{< voorbeeld kop="Voorbeeldcode Powerup winkel" >}}
{{< highlight javascript >}}
const powerupKosten = [10, 100, 500, 1000, 5000]

let geld = 0
const beschikbarePowerups = [true, true, true, true, true]
const clickerInkomsten = [1, 2, 4, 8, 32]

/* Koopt een Powerup, mits hier geld voor is en de powerup nog niet gekocht is. Update de pagina. */
function koopPowerup(tier) {
    if (beschikbarePowerups[tier] == false) {
        return
    }
    if (geld < powerupKosten[tier]) {
        return
    }

    beschikbarePowerups[tier] = false;
    geld = geld - powerupKosten[tier]
    clickerInkomsten[tier] = clickerInkomsten[tier] * 2

    /* Update de pagina! */
    verwijderPowerup(tier)
    updateGeldPerSeconde(berekenGeldPerSeconde())
    updateGeld(geld)
}

/* Verwijder de Powerup voor tier tier uit de lijst. */
function verwijderPowerup(tier) {
    const powerupDiv = document.getElementById("powerupWinkel")
    const tierDiv = powerupDiv.getElementsByClassName("tier" + tier)[0]

    tierDiv.remove()
}
{{< /highlight >}}
{{< /voorbeeld >}}

## Uitbreidingen

Allereerst: gefeliciteerd met het halen van het einde van deze instructie!

Er zijn een hoop uitbreidingen mogelijk op het basisconcept:
- de kosten van Clickers verhogen na elke aankoop;
- meerde Powerups per Clicker;
- meer Clickers toevoegen;

Hier kan je zelf mee aan de slag gaan
en natuurlijk extra hulp bij vragen. 

Laat ons eventueel weten of er nog andere uitbreidingen zijn die je in deze instructie zou willen zien!

## Top secret

{{< voorbeeld kop="De volledige voorbeeldcode" >}}

{{< highlight javascript >}}

    /* Een lijst met de icoontjes van de Clickers. */
    clickerIcons = [
        "https://pngimg.com/uploads/chef/chef_PNG54.png",
        /* tier 1 */
        /* tier 2 */
        /* tier 3 */
        /* tier 4 */
    ]
    
    /* De volgende functies beïnvloeden het uiterlijk van de pagina. */
    
    /* Voeg een nieuw icoontje toe aan de lijst met Actieve Clickers van tier tier */
    function voegActieveClickerToe(tier) {
        const actieveClickerDiv = document.getElementById("actieveClickers")
        const tierDiv = actieveClickerDiv.getElementsByClassName("tier" + tier)[0]
    
        const afbeelding = document.createElement("img")
        afbeelding.src = clickerIcons[tier]
        afbeelding.className = "icon"
    
        tierDiv.appendChild(afbeelding)
    }
    
    /* Verander het geld bedrag dat op de pagina word weergegeven naar hoeveelheid. */
    function updateGeld(hoeveelheid) {
        const geldH2 = document.getElementById("geld")
    
        geldH2.innerText = "Geld: " + hoeveelheid + "$"
    }
    
    /* Verander het geld per seconde bedrag dat op de pagina word weergegeven naar hoeveelheid. */
    function updateGeldPerSeconde(hoeveelheid) {
        const geldPerSecondeH2 = document.getElementById("geldPerSeconde")
    
        geldPerSecondeH2.innerText = "Geld per seconde: " + hoeveelheid + "$"
    }
    
    /* Verwijder de Powerup voor tier tier uit de lijst. */
    function verwijderPowerup(tier) {
        const powerupDiv = document.getElementById("powerupWinkel")
        const tierDiv = powerupDiv.getElementsByClassName("tier" + tier)[0]
    
        tierDiv.remove()
    }
    
    /* Vanaf hier focussen we op de functionaliteit! */
    
    /* De volgende gegevens veranderen niet. */
    const aantalClickerTiers = 5
    const clickerKosten = [1, 10, 50, 100, 500]
    const powerupKosten = [10, 100, 500, 1000, 5000]
    
    /* De volgende gevevens kunnen veranderen. */
    let geld = 0
    let maxAantalClickers = 14 /* Voor de basisfunctie ligt dit aan de grootte van je scherm. */
    const clickerInkomsten = [1, 2, 4, 8, 32]
    const aantalClickers = [0, 0, 0, 0, 0]
    const beschikbarePowerups = [true, true, true, true, true]
    
    /* Voegt 1$ toe en update de pagina */
    function onClickCookie() {
        geld = geld + 1
    
        updateGeld(geld) /* Update de pagina! */
    }
    
    /* Berekent het geld dat de Clickers genereren per seconde. */
    function berekenGeldPerSeconde() {
        let totaal = 0
    
        for (let tier = 0; tier < aantalClickerTiers; tier++) {
            const inkomsten = clickerInkomsten[tier]
            const aantal = aantalClickers[tier]
            totaal += inkomsten * aantal
        }
    
        return totaal
    }
    
    /* Koopt een Powerup, mits hier geld voor is en de Powerup nog niet gekocht is. Update de pagina. */
    function koopPowerup(tier) {
        if (beschikbarePowerups[tier] == false) {
            return
        }
        if (geld < powerupKosten[tier]) {
            return
        }
    
        beschikbarePowerups[tier] = false;
        geld = geld - powerupKosten[tier]
        clickerInkomsten[tier] = clickerInkomsten[tier] * 2
    
        /* Update de pagina! */
        verwijderPowerup(tier)
        updateGeldPerSeconde(berekenGeldPerSeconde())
        updateGeld(geld)
    }
    
    /* Koopt een Clicker, mits hier geld voor is en er nog ruimte is. Update de pagina. */
    function koopClicker(tier) {
        if (aantalClickers[tier] >= maxAantalClickers) {
            return
        }
        if (geld < clickerKosten[tier]) {
            return
        }
    
        geld = geld - clickerKosten[tier]
        aantalClickers[tier] = aantalClickers[tier] + 1
    
        /* Update de pagina! */
        voegActieveClickerToe(tier)
        updateGeldPerSeconde(berekenGeldPerSeconde())
        updateGeld(geld)
    }
    
    /* Voegt het geld van de Clickers toe aan het geld. Update de pagina. */
    function updateClickerGeld() {
        geld = geld + berekenGeldPerSeconde()
    
        /* Update de pagina! */
        updateGeld(geld)
    }
    
    /* Zorgt dat updateClickerGeld elke seconde activeert. */
    setInterval(updateClickerGeld, 1000)

{{< /highlight >}}

{{< /voorbeeld >}}

{{< licentie rel="http://creativecommons.org/licenses/by-nc-sa/4.0/">}}
