---
id: tutorial
title: "Guide: Intro till React"
layout: tutorial
sectionid: tutorial
permalink: tutorial/tutorial.html
redirect_from:
  - "docs/tutorial.html"
  - "docs/why-react.html"
  - "docs/tutorial-ja-JP.html"
  - "docs/tutorial-ko-KR.html"
  - "docs/tutorial-zh-CN.html"
---

Denna guide förutsätter inte några förkunskapar inom React.

## Innan vi startar guiden {#before-we-start-the-tutorial}

I denna guide kommer vi att bygga ett litet spel. **Du kan känna dig frestad att hoppa över det för att du inte bygger spel -- men ge det en chans.** Teknikerna du kommer lära dig i guiden är fundamentala för att bygga vilken React-applikation som helst, och genom att bemästra dem kommer du få djupare förståelse för React.

>Tips
>
>Denna guide är utformad för personer som föredrar att **lära genom att göra**. Om du föredrar att lära dig koncept från grunden, spana in vår [steg-för-steg-guide](/docs/hello-world.html). Du kanske finner att båda guiderna kompletterar varandra.

Denna guide är indelad i flera sektioner:

* [Uppsättning för guiden](#setup-for-the-tutorial) kommer ge dig **en utgångspunkt* att följa guiden.
* [Överblick](#overview) kommer lära dig **grundprinciperna** i React: komponenter, props och state.
* [Färdigställa Spelet](#completing-the-game) kommer lära dig **de vanligaste teknikerna** i React-utveckling.
* [Lägg till Time Travel](#adding-time-travel) kommer ge dig **en djupare insikt** i Reacts unika styrkor.

Du behöver inte fullfölja alla sektionerna på en gång för att få värdet ur denna guide. Försöka att komma så långt du kan -- även om det är en eller två sektioner.

### Vad är det vi bygger? {#what-are-we-building}

I denna guide kommer vi visa hur man bygger ett interaktivt tre-i-rad-spel med React.

Du kan se vad vi kommer att bygga här: **[Slutresultat](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)** Om koden inte ser vettig ut för dig, eller om du är obekant med kodens syntax, oroa dig inte! Målet med denna guide är att hjälpa dig förstå React och dess syntax.

Vi rekommenderar att du titta på tre-i-rad-spelet innan du fortsätter med guiden. En av funktionerna som du kommer märke är att det finns en numrerad lista till höger och spelplanen. Denna lista ger dig historiken över alla drag som skett i spelet och hållas uppdaterad under spelets gång.

Du kan stänga tre-i-rad-spelet när du känner dig bekant med det. Vi kommer att börja med en enklare mall i denna guide. Nästa steg är att sätta upp din miljö så att du kan börja bygga spelet.

### Förutsättningar {#prerequisites}

Vi antar att du är någorlunda bekant med HTML och JavaScript, men du borde kunna följa med även om du kommer från ett annat programmeringsspråk. Vi antar också att du är bekand med koncept inom programmering såsom funktioner, objekt, arrayer och i mindre utsträckning klasser.

Om du behöver se över JavaScript rekommenderar vi att du läser [denna guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript). Notera att vi också använder några funktioner från ES6 - en nyare version av JavaScript. I denna guide använder vi [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), och [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const). Du kan använda [Babel REPL](babel://es5-syntax-example) för att se vad ES6-kod kompileras till.

## Uppsättning för guiden {#setup-for-the-tutorial}

Det finns två sätt att fullfölja denna guide: du kan antingen skriva koden i din webbläsare, eller så kan du sätta upp en lokalt utvecklingsmiljö på din dator.

### Uppsättning Alterantiv 1: Skriv kod i webbläsaren {#setup-option-1-write-code-in-the-browser}

Det snabbaste sättet att komma igång!

Börja med att öppna denna **[Start-kod](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)** i en ny flik. Den nya fliken borde visa en tom tre-i-rad-spelplan och React-kod. Vi kommer att redigera React-koden i denna guide.

Du kan nu hoppa över det andra alternativet och gå direkt till sektionen [Överblick](#overview) för att få en överblick av React.

### Uppsättning Alternativ 2: Lokal utvecklingsmiljö {#setup-option-2-local-development-environment}

Detta är helt valfritt och inte ett krav för denna guide!

<br>

<details>

<summary><b>Valfritt: Instrukioner för att följa med lokalt med din föredragna textredigerare</b></summary>

Denna uppsättning kräver mer arbete men tillåter dig att fullfölja denna guide med en textredigerare du väljer själv. Gör följande steg:

1. Se till att du har en senare version av [Node.js](https://nodejs.org/en/) installerad.
2. Följ [instruktionerna för installations hos Create React App](/docs/create-a-new-react-app.html#create-react-app) för att skapa ett nytt projekt.

```bash
npx create-react-app my-app
```

3. Ta bort alla filerna i `src/`-katalogen för det nya projektet.

> Obs:
>
>**Ta inte bort hela `src`-katalogen, bara de ursprungilga källfilerna inuti den.** I nästa steg kommer vi att ersätta de ursprungliga källfilerna med kod för detta projekt.

```bash
cd my-app
cd src

# Om du använder Mac eller Linux:
rm -f *

# Eller, om du använder Windows:
del *

# Väl klart, byt tillbaka till projekt-katalogen
cd ..
```

4. Lägg till en fil vid namn `index.css` inuti `src/`-katalogen med [denna CSS-kod](https://codepen.io/gaearon/pen/oWWQNa?editors=0100).

5. Lägg till en fil vid namn `index.js` inuti `src/`-katalogen med [denna JS-kod](https://codepen.io/gaearon/pen/oWWQNa?editors=0010).

6. Lägg till följande tre rader i toppen av `index.js` som ligger inuti `src`-katalogen.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
```

Nu om du kör `npm start` i projekt-katalogen och öppnar `http://localhost:3000` i din webbläsare så borde du se en tom spelplan för tre-i-rad.

Vi rekommenderar att du följer [dessa instruktioner](https://babeljs.io/docs/editors/) för att konfigurera syntaxmarkering för din textredigerare.

</details>

### Hjälp, jag är fast! {#help-im-stuck}

Om du fastnar, kolla in [de tillgängliga support-resurserna](/community/support.html). I synnerhet så är [Reactiflux Chatten](https://discord.gg/0ZcbPKXt5bZjGY5n) en jättebra plats att få hjälp snabbt. Om du inte får ett svar, eller om du förblir fast, skapa en issue så hjälper vi dig.

## Överblick {#overview}

Nu när allt är uppsatt, lås oss skaffa och en överblick av React!

### Vad är React? {#what-is-react}

React är ett deklarativt, effektivt och flexibelt JavaScript-bibliotek för att bygga användargränssnitt. Det låter dig komponera ihop komplexa gränssnitt med hjälp av mindre och isolerade delar av kod som kallas "komponenter".

React har några olika typer av komponenter, men vi börjar med subklasser av `React.Component`:

```javascript
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping-lista för {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// Exempel på användning: <ShoppingList name="Mark" />
```

Vi kommer till de lite konstiga XML-liknande taggarna inom kort. Vi använder komponenter för att berätta för React vad vi vill se för något på skärmen. När vår data förändras så kommer React på ett effektivt sätt att uppdatera och rita om våra komponenter.

I detta exepmel så är ShoppingList en **React-komponent-klass** eller **React-komponent-typ**. En komponent kan ta emot parametrar, så kallade `props` (förkorning av engelskans "properties") och returnerar en hierarki av vyer att visa via `render`-metoden.

`render`-metoden returnerar en *beskrivning* av vad du vill se på skärmen. React tar beskrivningen och visar resultatet. Specifikt så returnerar `render` ett **React-element**, vilket är en lättviktigt beskrivning av vad som ska ritas ut. De flesta React-utvecklarna använder en speciell syntax kallas "JSX" som gör det enklare att skriva dessa strukturer. Koden `<div />` transformeras vid bygge till `React.createElement('div')`. Exemplet ovan är samma sak som:

```javascript
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... barn till h1 ... */),
  React.createElement('ul', /* ... barn till ul ... */)
);
```

[Se full version.](babel://tutorial-expanded-version)

Om du är intresserad så finns `createElement()` beskrivet i mer detalj i [API-dokumentationen](/docs/react-api.html#createelement), vi kommer dock inte använda den mer i denna guide. Istället kommer vi fortsätta använda oss av JSX.

JSX kommer med all funktionalitet som finns i JavaScript. Du skriva *vilket JavaScript-uttryck som helst* innanför krokiga paranteser i JSX. Varje React-element är ett JavaScript-objekt som du kan spara i en variabel och passa runt i ditt program.

`ShopplingList`-komponenten ovan rendrera bara ut inbyggda DOM-komponenter som `<div />` och `<li />`. Men man kan komponera och rendrera egna React-komponenter också. Till exempel så kan vi nu referera till hela shoppinglistan genom att skriva `<ShoppingList />`. Varje React-komponent är inkapslad och kan fungera oberoende av andra, detta gör att du kan bygga komplexa användargränssnit från enkla komponenter.

## Inspekterande av Start-koden {#inspecting-the-starter-code}

Om du kommer arbeta med denna guide **i din webbläsrare,** öppna denna kod i en ny flik: **[Start-kod](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)**.Om du kommer arbeta med denna guide **lokalt,** öppna istället `src/index.js` i projekt-katalogen (du har redan rört denna fil under [uppsättningen](#setup-option-2-local-development-environment)).

Denna Start-kod utgör basen för det vi ska bygga. Vi har tillhandahållit all CSS-styling så att du endast behöver fokusera på att lära dig React och programmera tre-i-rad-spelet.

Genom att inspektera koden så kommer du att se att vi har tre stycken React-komponenter:

* Square
* Board
* Game

Square-komponenten rendrera en enda `<button>` och Board-komponenten rendrerar nio Square-komponenter. Game-komponenten rendrerar en spelplan med platshållarvärden som vi kommer modifiera senare. Det finns för nävarande inga interaktiva komponenter.

### Passa data genom props {#passing-data-through-props}

För att bli varma i kläderna, låt oss prova att passa viss data från vår Board-komponent till vår Square-komponent.

We rekommenderar starkt att skriva kod för hand medan du arbetar dig igenom guiden och inte använda dig av kopiera/klistra in. Det kommer hjälpa dig utveckla muskelminne och bättre förståelse.

I Boards `renderSquare`-metod, ändra koden så att den passar en prop vid namn `value` till Square.

```js{3}
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;
  }
```

Change Square's `render` method to show that value by replacing `{/* TODO */}` with `{this.props.value}`:

```js{5}
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {this.props.value}
      </button>
    );
  }
}
```

Före:

![React Devtools](../images/tutorial/tictac-empty.png)

Efter: Du borde se en siffra i varje fyrkant på det rendrerade resultatet.

![React Devtools](../images/tutorial/tictac-numbers.png)

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/aWWQOG?editors=0010)**

Grattis! Du har just "passat en prop" från en föräldra-komponent, Board, till en barn-komponent, Square. Passande av props är hur information flöder genom en React-app, från föräldrar till barn.

### Skapa en interaktiv komponent {#making-an-interactive-component}

Låt oss fylla i Square-komponenten med ett "X" när vi klickar.
Ändra först button-taggen som returneras från Square-komponentens `render()`-funktion till detta:

```javascript{4}
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={function() { alert('click'); }}>
        {this.props.value}
      </button>
    );
  }
}
```

Om du nu klickar på en Square så borde du se en meddelanderuta i din webbläsare.

>OBS
>
>För att spara skrivande och undvika [förvirrande beteende av `this`](https://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/), kommer vi använda [arrow function syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) för eventhanterare här och längre nedan.
>
>```javascript{4}
>class Square extends React.Component {
>  render() {
>    return (
>      <button className="square" onClick={() => alert('click')}>
>        {this.props.value}
>      </button>
>    );
>  }
>}
>```
>
>Notera att med `onClick={() => alert('click')}` så passar vi *en funktion* som `onClick`-prop. React kommer endast att anropa denna funktion efter ett klick. Det är vanligt att glömma `() =>` och skriva `onClick={alert('click')}` istället, det skulle medföra att meddelanderutan skulle visas varje gång komponenten rendrerar om.

Som ett nästa steg så vill vi att Square-komponenten ska "komma ihåg" att den blivit klickad och fylla den med ett "X"-markering. För att "komma ihåg" saker använder komponenter sig av **state**.

React-komponenter kan spara state genom att sätta `this.state` i sin konstruktor. `this.state` ska anses vara privat till den React-komponent som definierat den. Låt oss spara det nuvarande värdet och Square i `this.state` och ändra det när Square blir klickad.

Först lägger vi till en konstruktor till klassen för att initialisera state:

```javascript{2-7}
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

>OBS
>
>I [JavaScript-klasser](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) så behöver man alltid kalla på `super` när man definierar en konstruktor i en subklass. Alla React-komponent-klasser som har en konstruktor ska börja den med ett anrop till `super(props)`.

Now we'll change the Square's `render` method to display the current state's value when clicked:

* Ersätt `this.props.value` med `this.state.value` inuti `<button>`-taggen.
* Ersätt eventhanterarern `onClick={...}` med `onClick={() => this.setState({value: 'X'})}`.
* Placera props `className` och `onClick` på egna rader för ökad läsbarhet.

Efter dessa ändringar så ser `<button>`-taggen, som returneras från Squares `render`-metod, ut så här:

```javascript{12-13,15}
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button
        className="square"
        onClick={() => this.setState({value: 'X'})}
      >
        {this.state.value}
      </button>
    );
  }
}
```

Genom att anropa `this.setState` från en `onClick`-hanterare i Squares `render`-metod så säger vi till React att rendrera om den Square-komponenten när helst dess `<button>` blir klickad. Efter uppdateringen så kommer den Sqaure-komponentens `this.state.value` att vara `'X'`, därför ser vi ett `X` på spelplanen. Om du klickar på vilken Square som helst så ska ett `X` dyka upp där.

När du anropar `setState` i en komponent så kommer React automatiskt att uppdatera alla barn-komponentern inuti den också.

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/VbbVLg?editors=0010)**)

### Utvecklarverktyg {#developer-tools}

Tillägget React Devtools för [Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) och [Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/) låter dig inspektera ett React-komponent-träd i din webbläsarens utvecklarverktyg.

<img src="../images/tutorial/devtools.png" alt="React Devtools" style="max-width: 100%">

React Devtools låter dig kontrollera React-komponenters props och state.

Efter installation av React Devtools så kan du högerklicka på något element på sidan och sedan klicka på "Inspektera" för att öpnna utvecklarverktygen. En React-flik kommer visas sist längst till höger.

**Dock så krävs det ytterligare några steg för att få det att fungera med CodePen:**

1. Logga in eller registrera dig, samt bekräfta din e-postaddress (krävs för att förhindra spam)
2. Klicka på knappen "Fork".
3. Klicka på "Change View" och välj sedan "Debug mode".
4. I den nya fliken som öppnas borde utvecklareverktygen ha en React-flik.

## Färdigställa Spelet {#completing-the-game}

Nu har vi de grundläggande byggstenarna för vårt tre-i-rad-spel. För att färdigställa spelet så behöver vi växla mellan att placera "X" och "O" på spelplanen, samt ett sätt att avgöra vinnaren.

### Lyfta upp state {#lifting-state-up}

För närvarande så håller varje Square-komponent reda på spelets state. För att kolla efter en vinnare så kommer vi upprätthålla värdet av samtliga nio Square-komponenter på en och samma plats.

Man kan tro att Board borde bara fråga varje Square för dess state. Trots att detta tillvägagångssätt är möjligt i React så avråder vi från det för att koden blir svår att förstå, mottaglig för buggar och svår att refaktorera. I stället är det bästa tillvägagånssättet att spara spelets state i föräldra-komponenten Board till skillnad för i varje individuell Squre. Board-komponenten kan tala om för varje Square vad som ska visas genom att passa en prop [precis som vi gjorde när vi passade en siffra till varje Square](#passing-data-through-props)

**För att samla data från flera barn, eller ha två barn-komponenter som kommunicerar med varandra, behöver du deklarera det delade statet i föräldra-komponent i stället. Föräldra-komponenten kan passa statet ner till barnen genom att använda props; detta håller barn-komponentenr i synk med varandra och med föräldra-komponenten.**

Lytfa state in till en föräldra-komponent är vanligt när React-komponenter blir refaktorerade -- låt oss ta tillfället i akt att testa det.

Lägg till en konstruktor till Board och sätt Boards initiala state till att innehålla en array av 9 null-värden som motsvarar de 9 Squares.

```javascript{2-7}
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }

  renderSquare(i) {
    return <Square value={i} />;
  }
```

När vi fyller i spelplanen senare, så kommer `this.state.squares`-arrayen att se ut något så här:

```javascript
[
  'O', null, 'X',
  'X', 'X', 'O',
  'O', null, null,
]
```

Boards `renderSquare`-metod ser för närvarande ut så här:

```javascript
  renderSquare(i) {
    return <Square value={i} />;
  }
```

I början så  [passade vi `value`-propen neråt](#passing-data-through-props) från Board för att visa siffror från 0 till 8 i varje Square. I ett annat, tidigare steg, ersatte vi siffrorna med en "X"-markering [bestämt av Squares egna state](#making-an-interactive-component). Det är därför som Square för närvarande ignorerear `value`-propen som passas till den av Board.

Vi kommer nu använda mekanismen att passa props igen. Vi kommer modifiera Board till att instruera varje individuell Square om dess nuvarande värde (`'X'`, `'O'`, or `null`). Vi har redan definierat `squares`-arrayen i Boards kontruktor och vi kommer modifiera Boards `renderSquare`-metod till att läsa från det:

```javascript{2}
  renderSquare(i) {
    return <Square value={this.state.squares[i]} />;
  }
```

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/gWWQPY?editors=0010)**

Varje Square kommer nu att ta emot en `value`-prop som kommer vara antingen `'X'`, `'O'`, or `null` för tomma rutor.

Härnäst behöver vi ändra vad som händer när en Square blir klickad. Board-komponenten håller nu reda på vilka rutor som är fyllda. Vi behöver skapa ett sätt för Square att uppdatera Boards state. Eftersom state ska anses vara privat till komponenten som definierar det, så kan vi inte uppdatera Boards state direkt från Square.

Istället passar vi ner en funktion från Board till Square och får Square att anropa på den funktionen när en ruta blir klickad. Vi kommer ändra `renderSquare`-metoden i Board också:

```javascript{5}
  renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }
```

>OBS
>
>Vi delar upp det returnerade elementet i flera rader för läsbarhet och lade till paranteser så att inte JavaScript lägger in semikolon efter `return` och har stönder vår kod.

Nu passar vi ner två props från Board till Square: `value` och `onClick`. `onClick`-propen är en funktion som Square can kalla när den blir klickad. Vi gör följande förändrar till Square:

* Ersätt `this.state.value` med `this.props.value` in Squares `render`-metod
* Ersätt `this.setState()` med `this.props.onClick()` i Squares `render`-metod
* Ta bort `constructor` från Square, eftersom Square inte längre håller koll på spelets state

Efter dessa förändras ser Square-komponenten ut så här:

```javascript{1,2,6,8}
class Square extends React.Component {
  render() {
    return (
      <button
        className="square"
        onClick={() => this.props.onClick()}
      >
        {this.props.value}
      </button>
    );
  }
}
```

När en Square blir klickad blir `onClick`-funktionen som tillhandahålls av Board kallad. Här är en genomgång av hur detta uppnås:

1. `onClick`-propen på den inbyggda DOM `<button>`-komponenten talar om för React att sätta upp en klick-eventlyssnare.
2. När en knapp blir klickad, kallar React på `onClick`-eventhanteraren som är definierad i Squares `render()`-metod.
3. Denna eventhanterare kallar på `this.props.onClick()`. Squares `onClick`-prop var specificerad av Board.
4. Eftersom Board passar `onClick={() => this.handleClick(i)}` till Square, kallar Square på `this.handleClick(i)` när den blir klickad.
5. Vi har inte definierat `handleClick()`-metoden än, så vår kod kraschar. Om du klickar en ruta nu borde du se en röd felskärm som säger något i stil med "this.handleClick is not a function".

>OBS
>
> DOM `<button>`-elementets `onClick` attribute har speciell innebörd för React eftersom det är en inbyggd komponent. För egna komponenter som Square så är namngivningen upp till dig. Vi skulle kunna ge vilket namn som helst till Squares `onClick`-prop eller Boards `handleClick`-metod, och koden skulle fungera likadant. I React är det konventionellt att använda namn som `on[Event]` för props som representerar event och `handle[Event]` för metoder som hanterar eventen.

Vi borde få ett fel när vi klickar på Square, eftersom vi inte har definierat `handleClick` än. Vi kommer nu lägga till `handleClick` till Board-klassen:

```javascript{9-13}
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }

  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = 'X';
    this.setState({squares: squares});
  }

  renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/ybbQJX?editors=0010)**

Efter dessa förändrar är vi tillbaka till att kunna klicka på Squares för att fylla dem, precis som vi kunde innan. Dock så är state nu sparat i Board-komponenten istället för i varje individeuall Square-komponent. När Boards state förändras rendreras Square-komponenterna om automatiskt. Genom att hålla state av alla rutor i Board-komponenten så möjliggör det Board att avgöra vinnaren i framtiden.

Eftersom Square-komponenterna inte längre håller state så får de värder från Board-komponenten och informerar Board-komponenten när de blivit klickade. I React-termer är Square-komponenterna nu **kontrollerade komponenter**. Board har full kontroll över dem.

Notera hur vi i `handleClick` kallar på `.slice()` för att skapa en kopia av `squares`-arrayen som vi kan modifiera, istället för att modifiera en befintlig array. Vi kommer förklara varför vi skapar en kopia av `squares`-arrayen i nästa sektion.

### Varför oföränderlighet är viktigt {#why-immutability-is-important}

I föregående kod-exempel föreslog vi att du skulle använda `.slice()`-metoden för att skapa en kopia av `squares`-arrayen för att modifiera, istället för att modifiera en befintlig array. Vi ska nu diskutera oföränderlighet och varför oföränderlighet är viktigt att lära.

Det finns generellt två tillvägagångssätt att förändra data. Det första är att *mutera* datan genom att direkt ändra datans värden. Det andra är att ersätta datan med en ny kopia som har de önskade förändringarna.

#### Data-ändring med mutation {#data-change-with-mutation}
```javascript
var player = {score: 1, name: 'Jeff'};
player.score = 2;
// Nu är player {score: 2, name: 'Jeff'}
```

#### Data-ändring utan mutation {#data-change-without-mutation}
```javascript
var player = {score: 1, name: 'Jeff'};

var newPlayer = Object.assign({}, player, {score: 2});
// Nu är player oförändrad, men newPlayer är {score: 2, name: 'Jeff'}

// Eller om du använder "object spread syntax proposal", så kan du skriva:
// var newPlayer = {...player, score: 2};
```

Slutresultat är detsamma, men genom att inte mutera (eller ändra den underliggande datan) direkt så får vi flera fördelar som beskrivs nedan.

#### Komplexa funktioner blir enkla  {#complex-features-become-simple}

Oföränderlighet gör komplexa funktion mycket enklare att implementera. Senare i denna guide kommer vi implementera en "time travel"-funktion som kommer tillåta oss att gå igenom tre-i-rad-spelets historik och "hoppa tillbaka" till ett föregående drag. Denna funktionalitet är inte specifik för spel -- förmågan att ångra och göra om vissa åtgärder är ett vanligt krav i applikationer. Genom att undvika direkt mutation av data så låter det oss behålla föregående version av spelets historik intakt och återanvända de i ett senare skede.

#### Upptäcka ändringar {#detecting-changes}

Att upptäcka ändringar i muterbara objekt är svårt eftersom de modifieras direkt. För att lyckas upptäcka ändringar så krävs det att man jämför det muterade objektet med föregående kopia av sig själv och hela objektets träd måste då traverseras.

Att upptäcka ändringar i oföränderliga objekt är betydligt enklare. Om ett oföränderligt objekt som refereras till skiljer sig från det föregående, så har objektet ändrats.

#### Avgöra när man rendrerar om i React {#determining-when-to-re-render-in-react}

Den primära fördelen med oföränderlighet är att det hjälper dig bygga _rena komponenter_ i React. Oföränderlig data kan med lätthet avgöra om det skett ändringar vilket hjälper med att avgöra när en komponent behöver rendreras om.

Du kan lära mer om `shouldComponentUpdate()` och hur du kan bygga *rena komponenter* genom att läsa [Optimera prestanda](/docs/optimizing-performance.html#examples).

### Funktion-komponenter {#function-components}

Vi kommer nu ändra Square till att bli en **funktion-komponent**.

I React är **funktion-komponenter** ett simplare sätt att skriva komponenter på, som endast har en `render`-metod och inte innehar eget state. Istället för att definiera en klass som utökar `React.Component` kan vi definiera en funktion som tar `props` som input och returnerar det som ska rendreras. Funktion-komponenter är inte lika tradig att skriva som klasser och många komponenter kan uttryckas på detta sätt.

Ersätt Square-klassen med denna funktion:

```javascript
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

Vi har ändrat `this.props` till `props` på båda platser det förekommer.

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/QvvJOv?editors=0010)**

>OBS
>
>När vi modifierade Square till att bli en funktion-komponent ändrade vi också `onClick={() => this.props.onClick()}` till det kortare `onClick={props.onClick}` (notera avsaknade av paranteser på *båda* sidorna).

### Turas om {#taking-turns}

Vi behöver åtgärda en uppenbar defekt i vårt tre-i-rad-spel: "O" kan inte markeras på spelplanen.

Vi sätter första speldraget till att vara "X" som standard. Vi kan sätta standardvärdet genom att modifiera det initiala statet i konstruktorn i vår Board.

```javascript{6}
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
      xIsNext: true,
    };
  }
```

Varje gång en spelare gör ett drag kommer `xIsNext` (en boolesk datatyp) att växlas för att avgöra vilken spelares tur det är här näst samt spara spelets state. Vi uppdaterar Boars `handleClick`-funktion till att växla värdet av `xIsNext`:

```javascript{3,6}
  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }
```

Med denna ändringar turas "X" och "O" om. Testa!

Låt oss också ändra "status"-texten i Boards `render` för att visa vilken spelare som gör nästa drag.

```javascript{2}
  render() {
    const status = 'Nästa spelare: ' + (this.state.xIsNext ? 'X' : 'O');

    return (
      // resten är oförändrat
```

Efter applicering av dessa förändringar borde du ha följande Board-komponent:

```javascript{6,11-16,29}
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
      xIsNext: true,
    };
  }

  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }

  renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }

  render() {
    const status = 'Nästa spelare: ' + (this.state.xIsNext ? 'X' : 'O');

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/KmmrBy?editors=0010)**

### Utse en vinnare {#declaring-a-winner}

När vi nu visar vilken spelare tur det är här näst borde vi också visa när spelet är vunnet och det inte längre finns några fler drag att göra. Kopiera denna hjälp-funktion och klistra in den i slutet av filen:

```javascript
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

Givet en array av 9 rutor kommer denna funktion att kolla efter en vinntare och returnera `'X'`, `'O'`, or `null` i förekommande fall.

Vi kommer kalla `calculateWinner(squares)` i Boards `render`-funktion för att kolla om en spelare har vunnit. Om en spelare har vunnit kan vi visa en text såsom "Vinnare: X" eller "Vinnare: O". Vi kommer ersätta deklarationen av `status` i BOards `render`-funktion med denna kod:

```javascript{2-8}
  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Vinnare: ' + winner;
    } else {
      status = 'Nästa spelare: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      // resten är oförändrat
```

Vi kan nu ändra Boards `handleClick`-funktion att returnera tidigt genom att ignorera klick om någon har vunnit spelet eller om en ruta redan är markerad:

```javascript{3-5}
  handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }
```

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/LyyXgK?editors=0010)**

Grattis! Du har nu ett fungerande tre-i-rad-spel. Du har också lärt det grunderna i React. So *du* är troligen den riktiga vinnaren här.

## Adding Time Travel {#adding-time-travel}

As a final exercise, let's make it possible to "go back in time" to the previous moves in the game.

### Storing a History of Moves {#storing-a-history-of-moves}

If we mutated the `squares` array, implementing time travel would be very difficult.

However, we used `slice()` to create a new copy of the `squares` array after every move, and [treated it as immutable](#why-immutability-is-important). This will allow us to store every past version of the `squares` array, and navigate between the turns that have already happened.

We'll store the past `squares` arrays in another array called `history`. The `history` array represents all board states, from the first to the last move, and has a shape like this:

```javascript
history = [
  // Before first move
  {
    squares: [
      null, null, null,
      null, null, null,
      null, null, null,
    ]
  },
  // After first move
  {
    squares: [
      null, null, null,
      null, 'X', null,
      null, null, null,
    ]
  },
  // After second move
  {
    squares: [
      null, null, null,
      null, 'X', null,
      null, null, 'O',
    ]
  },
  // ...
]
```

Now we need to decide which component should own the `history` state.

### Lifting State Up, Again {#lifting-state-up-again}

We'll want the top-level Game component to display a list of past moves. It will need access to the `history` to do that, so we will place the `history` state in the top-level Game component.

Placing the `history` state into the Game component lets us remove the `squares` state from its child Board component. Just like we ["lifted state up"](#lifting-state-up) from the Square component into the Board component, we are now lifting it up from the Board into the top-level Game component. This gives the Game component full control over the Board's data, and lets it instruct the Board to render previous turns from the `history`.

First, we'll set up the initial state for the Game component within its constructor:

```javascript{2-10}
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      xIsNext: true,
    };
  }

  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}
```

Next, we'll have the Board component receive `squares` and `onClick` props from the Game component. Since we now have a single click handler in Board for many Squares, we'll need to pass the location of each Square into the `onClick` handler to indicate which Square was clicked. Here are the required steps to transform the Board component:

* Delete the `constructor` in Board.
* Replace `this.state.squares[i]` with `this.props.squares[i]` in Board's `renderSquare`.
* Replace `this.handleClick(i)` with `this.props.onClick(i)` in Board's `renderSquare`.

The Board component now looks like this:

```javascript{17,18}
class Board extends React.Component {
  handleClick(i) {
    const squares = this.state.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }

  renderSquare(i) {
    return (
      <Square
        value={this.props.squares[i]}
        onClick={() => this.props.onClick(i)}
      />
    );
  }

  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

We'll update the Game component's `render` function to use the most recent history entry to determine and display the game's status:

```javascript{2-11,16-19,22}
  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);

    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
```

Since the Game component is now rendering the game's status, we can remove the corresponding code from the Board's `render` method. After refactoring, the Board's `render` function looks like this:

```js{1-4}
  render() {
    return (
      <div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
```

Finally, we need to move the `handleClick` method from the Board component to the Game component. We also need to modify `handleClick` because the Game component's state is structured differently. Within the Game's `handleClick` method, we concatenate new history entries onto `history`.

```javascript{2-4,10-12}
  handleClick(i) {
    const history = this.state.history;
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares,
      }]),
      xIsNext: !this.state.xIsNext,
    });
  }
```

>Note
>
>Unlike the array `push()` method you might be more familiar with, the `concat()` method doesn't mutate the original array, so we prefer it.

At this point, the Board component only needs the `renderSquare` and `render` methods. The game's state and the `handleClick` method should be in the Game component.

**[View the full code at this point](https://codepen.io/gaearon/pen/EmmOqJ?editors=0010)**

### Showing the Past Moves {#showing-the-past-moves}

Since we are recording the tic-tac-toe game's history, we can now display it to the player as a list of past moves.

We learned earlier that React elements are first-class JavaScript objects; we can pass them around in our applications. To render multiple items in React, we can use an array of React elements.

In JavaScript, arrays have a [`map()` method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) that is commonly used for mapping data to other data, for example:

```js
const numbers = [1, 2, 3];
const doubled = numbers.map(x => x * 2); // [2, 4, 6]
```

Using the `map` method, we can map our history of moves to React elements representing buttons on the screen, and display a list of buttons to "jump" to past moves.

Let's `map` over the `history` in the Game's `render` method:

```javascript{6-15,34}
  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);

    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });

    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={(i) => this.handleClick(i)}
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{moves}</ol>
        </div>
      </div>
    );
  }
```

**[View the full code at this point](https://codepen.io/gaearon/pen/EmmGEa?editors=0010)**

For each move in the tic-tac-toes's game's history, we create a list item `<li>` which contains a button `<button>`. The button has a `onClick` handler which calls a method called `this.jumpTo()`. We haven't implemented the `jumpTo()` method yet. For now, we should see a list of the moves that have occurred in the game and a warning in the developer tools console that says:

>  Warning:
>  Each child in an array or iterator should have a unique "key" prop. Check the render method of "Game".

Let's discuss what the above warning means.

### Picking a Key {#picking-a-key}

When we render a list, React stores some information about each rendered list item. When we update a list, React needs to determine what has changed. We could have added, removed, re-arranged, or updated the list's items.

Imagine transitioning from

```html
<li>Alexa: 7 tasks left</li>
<li>Ben: 5 tasks left</li>
```

to

```html
<li>Ben: 9 tasks left</li>
<li>Claudia: 8 tasks left</li>
<li>Alexa: 5 tasks left</li>
```

In addition to the updated counts, a human reading this would probably say that we swapped Alexa and Ben's ordering and inserted Claudia between Alexa and Ben. However, React is a computer program and does not know what we intended. Because React cannot know our intentions, we need to specify a *key* property for each list item to differentiate each list item from its siblings. One option would be to use the strings `alexa`, `ben`, `claudia`. If we were displaying data from a database, Alexa, Ben, and Claudia's database IDs could be used as keys.

```html
<li key={user.id}>{user.name}: {user.taskCount} tasks left</li>
```

When a list is re-rendered, React takes each list item's key and searches the previous list's items for a matching key. If the current list has a key that didn't exist before, React creates a component. If the current list is missing a key that existed in the previous list, React destroys the previous component. If two keys match, the corresponding component is moved. Keys tell React about the identity of each component which allows React to maintain state between re-renders. If a component's key changes, the component will be destroyed and re-created with a new state.

`key` is a special and reserved property in React (along with `ref`, a more advanced feature). When an element is created, React extracts the `key` property and stores the key directly on the returned element. Even though `key` may look like it belongs in `props`, `key` cannot be referenced using `this.props.key`. React automatically uses `key` to decide which components to update. A component cannot inquire about its `key`.

**It's strongly recommended that you assign proper keys whenever you build dynamic lists.** If you don't have an appropriate key, you may want to consider restructuring your data so that you do.

If no key is specified, React will present a warning and use the array index as a key by default. Using the array index as a key is problematic when trying to re-order a list's items or inserting/removing list items. Explicitly passing `key={i}` silences the warning but has the same problems as array indices and is not recommended in most cases.

Keys do not need to be globally unique; they only need to be unique between components and their siblings.


### Implementing Time Travel {#implementing-time-travel}

In the tic-tac-toe game's history, each past move has a unique ID associated with it: it's the sequential number of the move. The moves are never re-ordered, deleted, or inserted in the middle, so it's safe to use the move index as a key.

In the Game component's `render` method, we can add the key as `<li key={move}>` and React's warning about keys should disappear:

```js{6}
    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li key={move}>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });
```

**[View the full code at this point](https://codepen.io/gaearon/pen/PmmXRE?editors=0010)**

Clicking any of the list item's buttons throws an error because the `jumpTo` method is undefined. Before we implement `jumpTo`, we'll add `stepNumber` to the Game component's state to indicate which step we're currently viewing.

First, add `stepNumber: 0` to the initial state in Game's `constructor`:

```js{8}
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      stepNumber: 0,
      xIsNext: true,
    };
  }
```

Next, we'll define the `jumpTo` method in Game to update that `stepNumber`. We also set `xIsNext` to true if the number that we're changing `stepNumber` to is even:

```javascript{5-10}
  handleClick(i) {
    // this method has not changed
  }

  jumpTo(step) {
    this.setState({
      stepNumber: step,
      xIsNext: (step % 2) === 0,
    });
  }

  render() {
    // this method has not changed
  }
```

We will now make a few changes to the Game's `handleClick` method which fires when you click on a square.

The `stepNumber` state we've added reflects the move displayed to the user now. After we make a new move, we need to update `stepNumber` by adding `stepNumber: history.length` as part of the `this.setState` argument. This ensures we don't get stuck showing the same move after a new one has been made.

We will also replace reading `this.state.history` with `this.state.history.slice(0, this.state.stepNumber + 1)`. This ensures that if we "go back in time" and then make a new move from that point, we throw away all the "future" history that would now become incorrect.

```javascript{2,13}
  handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares
      }]),
      stepNumber: history.length,
      xIsNext: !this.state.xIsNext,
    });
  }
```

Finally, we will modify the Game component's `render` method from always rendering the last move to rendering the currently selected move according to `stepNumber`:

```javascript{3}
  render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);

    // the rest has not changed
```

If we click on any step in the game's history, the tic-tac-toe board should immediately update to show what the board looked like after that step occurred.

**[View the full code at this point](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)**

### Wrapping Up {#wrapping-up}

Congratulations! You've created a tic-tac-toe game that:

* Lets you play tic-tac-toe,
* Indicates when a player has won the game,
* Stores a game's history as a game progresses,
* Allows players to review a game's history and see previous versions of a game's board.

Nice work! We hope you now feel like you have a decent grasp on how React works.

Check out the final result here: **[Final Result](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)**.

If you have extra time or want to practice your new React skills, here are some ideas for improvements that you could make to the tic-tac-toe game which are listed in order of increasing difficulty:

1. Display the location for each move in the format (col, row) in the move history list.
2. Bold the currently selected item in the move list.
3. Rewrite Board to use two loops to make the squares instead of hardcoding them.
4. Add a toggle button that lets you sort the moves in either ascending or descending order.
5. When someone wins, highlight the three squares that caused the win.
6. When no one wins, display a message about the result being a draw.

Throughout this tutorial, we touched on React concepts including elements, components, props, and state. For a more detailed explanation of each of these topics, check out [the rest of the documentation](/docs/hello-world.html). To learn more about defining components, check out the [`React.Component` API reference](/docs/react-component.html).
