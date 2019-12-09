---
id: thinking-in-react
title: Tänka i React
permalink: docs/thinking-in-react.html
redirect_from:
  - 'blog/2013/11/05/thinking-in-react.html'
  - 'docs/thinking-in-react-zh-CN.html'
prev: composition-vs-inheritance.html
---

React är, enligt oss själva, den bästa metoden för att bygga stora och snabba webbappar med JavaScript. Det har anpassat sig väldigt väl för oss på Facebook och Instagram när vi vuxit.

En av de många fantastiska sakerna med React är hur det får dig att tänka på appar medan du bygger dem. I det här dokumentet kommer vi att gå igenom tankeprocessen vid byggande av en sökbar tabell med produktdata med hjälp av React.

## Börja med en mockup {#start-with-a-mock}

Tänk dig att vi redan har ett JSON API och en mockup från vår designer. Mockupen ser ut så här:

![Mockup](../images/blog/thinking-in-react-mock.png)

Vårt JSON API returnerar lite data som ser ut så här:

```
[
  {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
  {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
  {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
  {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
  {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
  {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
];
```

## Steg 1: Dela upp användargränssnittet i en komponenthierarki {#step-1-break-the-ui-into-a-component-hierarchy}

Det första steget du vill ta är att rita rutor runt varje komponent (och subkomponent) i mockupen och namnge dem. Om du jobbar med en designer kanske de redan har gjort detta, så prata med dem! Deras lagernamn i Photoshop kan förmodligen användas som namn på dina React-komponenter!

Men hur vet man vad som ska vara en egen komponent? Använd samma metod som när man bör skapa en ny funktion eller ett nytt objekt. En sådan teknik är [single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), det vill säga att en komponent egentligen bara ska göra en sak. Om den börjar växa bör den delas upp i mindre subkomponenter.

Eftersom man ofta visar användare JSON-data, kommer du upptäcka att om din datamodell är korrekt uppbyggd kommer den att gå hand-i-hand med ditt användargränssnitt (och därav även med din komponentstruktur). Det beror på att användargränssnitt och datamodeller tenderar att hålla sig till samma *informationsarkitektur*. Separera ditt användargränssnitt i komponenter, där varje komponent matchar varsin del av din datamodell.

![Komponentdiagram](../images/blog/thinking-in-react-components.png)

Här ser du att vi har fem komponenter i vår app. Vi har kursiverat datan varje komponent representerar.

  1. **`FilterableProductTable` (orange):** innehåller allt i exemplet
  2. **`SearchBar` (blå):** tar emot all *användarinput*
  3. **`ProductTable` (grön):** visar och filtrerar *datalistan* baserat på *användarinput*
  4. **`ProductCategoryRow` (turkos):** visar en titel för varje *kategori*
  5. **`ProductRow` (röd):** visar en rad för varje *produkt*

Ifall du tittar på `ProductTable` ser du att tabellhuvudet (som innehåller "Name"- och "Price"-etiketterna) inte är en separat komponent. Det här är en smaksak, och det går att argumentera för båda alternativen. I det här exemplet har vi låtit det vara en del av `ProductTable` eftersom det har en del i att rendera *datalistan*, vilket `ProductTable` ansvarar för. Om däremot tabellhuvudet skulle växa och bli mer komplext (t.ex. om vi skulle lägga till möjligheten att sortera) vore det logiskt att göra det till en egen `ProductTableHeader`-komponent.

Nu när vi har identifierat komponenterna i vår mockup är det dags att ordna in dem i en hierarki. Komponenter som visas inuti en annan komponent i mockupen bör visas som en undernivå i hierarkin:

  * `FilterableProductTable`
    * `SearchBar`
    * `ProductTable`
      * `ProductCategoryRow`
      * `ProductRow`

## Steg 2: Bygg en statisk version i React {#step-2-build-a-static-version-in-react}

<p data-height="600" data-theme-id="0" data-slug-hash="BwWzwm" data-default-tab="js" data-user="lacker" data-embed-version="2" class="codepen">Se Pennan <a href="https://codepen.io/gaearon/pen/BwWzwm">Thinking In React: Step 2</a> på <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

Nu när du har en komponenthierarki är det dags att implementera din app. Det lättaste sättet är att bygga en version som använder din datamodell och renderar användargränssnittet utan någon interaktivitet. Det är bäst att dela upp de här processerna eftersom det krävs mycket skrivande men inget tänkande att bygga en statisk version, medan att lägga till interaktivitet kräver en hel del tänkande men inte så mycket skrivande. Vi kommer att se varför det är så längre fram.

För att bygga en statisk version av din app som renderar din datamodell, behöver du bygga komponenter som återanvänder andra komponenter och skickar data mellan dessa via *props*. *props* är ett sätt att skicka data från en överordnad komponent till en underordnad. Om du är bekant med konceptet med *state*, **använd inte state över huvud taget** för att bygga den här statiska versionen. State är reserverat enbart för interaktivitet, med andra ord, data som förändras över tid. Eftersom det här är en statisk version av appen kommer du inte att behöva det.

Du kan bygga uppifrån och ned eller nedifrån och upp. Med andra ord, du kan antingen börja att bygga komponenterna högre upp i hierarkin (t.ex. att börja med `FilterableProductTable`), eller med dem längre ned i den (`ProductRow`). I mer enkla exempel är det oftast lättare att gå uppifrån och ned, och på större projekt gå nedifrån och upp, och skriva tester medan du bygger.

I slutet av det här steget kommer du att ha ett bibliotek av återanvändbara komponenter som renderar din datamodell. Komponenterna kommer bara att ha en `render()`-metod eftersom det är en statisk version av din app. Komponenten i toppen av hierarkin (`FilterableProductTable`) kommer att ta emot din datamodell som en attribut. Om du gör en ändring i din underliggande datamodell och anropar `ReactDOM.render()` igen kommer användargränssnittet att uppdateras. Då kommer du att se hur ditt användargränssnitt uppdateras, och var någonstans man kan göra ändringar. React:s **envägsdataflöde** (även kallad *envägsbindning*) håller allting modulärt och snabbt.

Se [React-dokumentationen](/docs/) om du behöver hjälp med att genomföra det här steget.

### En kortfattad förklaring: Props vs state {#a-brief-interlude-props-vs-state}

Det finns två typer av "modell"-data i React: props och state. Det är viktigt att förstå skillnaden mellan de två; skumma igenom [den officiella React-dokumentationen](/docs/state-and-lifecycle.html) om du inte är säker på vad skillnaden är. Se även [Vanliga frågor: Vad är skillnaden mellan state och props?](/docs/faq-state.html#what-is-the-difference-between-state-and-props)

## Steg 3: Identifiera den minimala (men kompletta) representationen av användargränssnittets olika state:s {#step-3-identify-the-minimal-but-complete-representation-of-ui-state}

För att göra ditt användargränssnitt interaktivt begöver du kunna orsaka förändringar i din datamodell. React åstadkommer detta med **state**.

För att bygga din app på rätt sätt behöver du först komma på vilket som är det minimala ändringsbara state som din app behöver. Nyckeln här är [DRY: *Don't Repeat Yourself*](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself). Tänk ut den absolut minimala återspeglingen av de olika tillstånd din applikation kommer att befinna sig i, och beräkna allt annat on-demand. Om du till exempel bygger en Att göra-lista, håll då en array av Att göra-punkterna till hands. När du vill rendera antalet Att göra-punkter, ta då längden på arrayen med Att göra-punkter, istället för att ha en separat variabel för antalet punkter.

Fundera över alla bitar av data i vår exempelapp. Vi har:

  * Den ursprungliga listan med produkter
  * Sökfrasen som användaren angett
  * Värdet på kryssrutan
  * Den filtrerade listan av produkter

Vi går igenom en efter en och räknar ut vilken av dem som är state. Ställ tre frågor om varje bit av data:

  1. Kommer den inmatad från en överordnad komponent via props? I så fall är det troligen inte state.
  2. Förblir den oförändrad över tid? I så fall är det troligen inte state.
  3. Kan den beräknas baserat utifrån något annat state eller props i din komponent? Då är det inte state.

Den ursprungliga listan av produkter är inmatad som props, så det är inte state. Sökfrasen och kryssrutan verkar vara state eftersom de förändras över tid, och inte kan beräknas utifrån någonting. Slutligen, den filtrerade listan av produkter är inte state eftersom den kan beräknas genom att kombinera den ursprungliga listan av produkter med sökfrasen och kryssrutans värde.

Så äntligen, vårt state är:

  * Sökfrasen som användaren angett
  * Värdet på kryssrutan

## Steg 4: Identifiera var ditt state bör finnas {#step-4-identify-where-your-state-should-live}

<p data-height="600" data-theme-id="0" data-slug-hash="qPrNQZ" data-default-tab="js" data-user="lacker" data-embed-version="2" class="codepen">Se Pennan <a href="https://codepen.io/gaearon/pen/qPrNQZ">Thinking In React: Step 4</a> på <a href="https://codepen.io">CodePen</a>.</p>

Okej, så vi har identifierat vad det minimala state:et för vår app är. Nästa steg är att identifiera vilka komponenter som muterar, eller *äger*, state:et.

Kom ihåg: React handlar om envägsdataflöde ned genom komponenthierarkin. Det kanske till en början inte är helt uppenbart vilken komponent som bör äga visst state. **Det här är ofta det svåraste för nykomlingar att förstå,** så vi följer de här stegen för att räkna ut det:

För varje bit av state i din applikation:

  * Identifiera varje komponent som renderar någonting baserat på state:et.
  * Hitta en gemensam ägar-komponent (en enda komponent över alla de komponenter som behöver state:et i hierarkin).
  * Antingen bör den gemensamma ägar-komponenter eller en annan komponent högre upp i hierarkin äga state:et.
  * Om du inte kan hitta en komponent som rent logiskt skulle kunna hålla state:et, skapa då en ny komponent enbart för att hålla state:et och lägg till den någonstans i hierarkin ovanför den gemensamma ägar-komponenten.

Låt oss gå igenom den här strategin för vår applikation:

  * `ProductTable` behöver kunna filtrera produktlistan baserat på state, och `SearchBar` behöver kunna visa sökfrasen och state för ikryssad kryssruta.
  * Den gemensamma ägar-komponenten är `FilterableProductTable`.
  * Det är logiskt för sökfrasen och det ikryssade värdet att höra hemma i `FilterableProductTable`

Coolt! Så nu har vi kommit fram till att vårt state hör hemma i `FilterableProductTable`. Först, lägg till en instance property `this.state = {filterText: '', inStockOnly: false}` på `FilterableProductTable`:s konstruktor för att återspegla det initiella state:et hos vår applikation. Därefter, skicka vidare `filterText` och `inStockOnly` till `ProductTable` och `SearchBar` som en prop. Till sist, använd dessa props för att filtrera raderna i `ProductTable`, och sätt värdena på formulärfälten i `SearchBar`.

Nu börjar det synas hur din applikation kommer att bete sig: sätt `filterText` till `"ball"` och ladda om din app. Då ser du att datatabellen har uppdaterats korrekt.

## Steg 5: Lägg till dataflöde i motsatt riktning {#step-5-add-inverse-data-flow}

<p data-height="600" data-theme-id="0" data-slug-hash="LzWZvb" data-default-tab="js,result" data-user="rohan10" data-embed-version="2" data-pen-title="Thinking In React: Step 5" class="codepen">Se Pennan <a href="https://codepen.io/gaearon/pen/LzWZvb">Thinking In React: Step 5</a> på <a href="https://codepen.io">CodePen</a>.</p>

Hittills har vi byggt en app som renderas korrekt, där props och state flödar nedåt genom hierarkin. Nu är det dags att lägga till stöd för dataflöde åt det motsatta hållet: formulärkomponenterna djupt nere i hierarkin behöver uppdatera state:et i `FilterableProductTable`.

React gör att den här typen av dataflöde görs uttryckligen för att hjälpa dig att förstå hur ditt program fungerar, men det kräver lite mer skrivande än den traditionella tvåvägsbindningen.

Om du testar att skriva något eller kryssa i rutan i den nuvarande versionen av vår exempelapp, ser du att React ignorerar din input. Det är medvetet gjort så här, eftersom vi sätter `value`-attributen hos vår `input` till att alltid vara lika med det `state` som matas in från `FilterableProductTable`.

Fundera en stund på vad vi vill ska hända. Vi vill vara säkra på att så fort användaren ändrar formuläret, så ska state:et uppdateras för att återspegla användarens input. Eftersom komponenter bara bör uppdatera sitt eget state, kommer `FilterableProductTable` att ge `SearchBar` en callback som anropas varje gång state:et ska uppdateras. Vi kan använda oss av `onChange`-eventet på inputfälten för att bli informerade om när det sker. De callbacks som matas in av `FilterableProductTable` kommer att anropa `setState()`, och därigenom uppdateras appen.

## Det var allt {#and-thats-it}

Förhoppningsvis har detta gett dig en inblick i hur du bör tänka när du bygger komponenter och applikationer med React. Det kanske visserligen är lite mer skrivande än vad du är van vid, men kom ihåg att man läser kod mycket oftare än man skriven den, och det är inte lika svårt att läsa den här slags modulära och uttryckliga koden. När du börjar bygga stora komponentbibliotek kommer du att uppskatta denna uttrycklighet och moduläritet, och med återanvändning av kod kommer dina kodrader att börja minska. :)
