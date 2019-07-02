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

* [Uppsättning för guiden](#setup-for-the-tutorial) kommer ge dig *en utgångspunkt* för att följa guiden.
* [Överblick](#overview) kommer lära dig **grundprinciperna** i React: komponenter, props och state.
* [Färdigställa spelet](#completing-the-game) kommer lära dig **de vanligaste teknikerna** inom React-utveckling.
* [Lägg till Time Travel](#adding-time-travel) kommer ge dig **en djupare insikt** i Reacts unika styrkor.

Du behöver inte fullfölja alla sektionerna på en och samma gång för att få värde ur denna guide. Försöka att komma så långt du kan -- även om det är en eller två sektioner.

### Vad är det vi bygger? {#what-are-we-building}

I denna guide kommer vi visa hur man bygger ett interaktivt tre-i-rad-spel med React.

Du kan se vad vi kommer att bygga här: **[Slutresultat](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)** Oroa dig inte om du inte begriper dig på koden eller om du är obekant med kodens syntax! Målet med denna guide är att hjälpa dig förstå React och dess syntax.

Vi rekommenderar att du tittar på tre-i-rad-spelet innan du fortsätter med guiden. En av funktionerna som du kommer märka är att det finns en numrerad lista till höger och spelplanen. Denna lista ger dig historiken över alla drag som skett i spelet och hålls uppdaterad under spelets gång.

Du kan stänga tre-i-rad-spelet när du känner dig bekant med det. Vi kommer att börja med en enklare mall i denna guide. Nästa steg är att sätta upp din miljö så att du kan börja bygga spelet.

### Förutsättningar {#prerequisites}

Vi antar att du är någorlunda bekant med HTML och JavaScript, men du borde kunna följa med även om du kommer från ett annat programmeringsspråk. Vi antar också att du är bekant med koncept inom programmering såsom funktioner, objekt, arrayer och i mindre utsträckning klasser.

Om du behöver se över dina JavaScript-kunskaper rekommenderar vi att du läser [denna guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript). Notera att vi också använder några funktioner från ES6 -- en nyare version av JavaScript. I denna guide använder vi [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), och [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const). Du kan använda [Babel REPL](babel://es5-syntax-example) för att se vad ES6-kod kompileras till.

## Uppsättning för guiden {#setup-for-the-tutorial}

Det finns två sätt att fullfölja denna guide: du kan antingen skriva koden i din webbläsare, eller så kan du sätta upp en lokal utvecklingsmiljö på din dator.

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

1. Se till att du har en nyare version av [Node.js](https://nodejs.org/en/) installerad.
2. Följ [instruktionerna för installation av Create React App](/docs/create-a-new-react-app.html#create-react-app) för att skapa ett nytt projekt.

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

Om du nu kör `npm start` i projekt-katalogen och öppnar `http://localhost:3000` i din webbläsare så borde du se en tom spelplan för tre-i-rad.

Vi rekommenderar att du följer [dessa instruktioner](https://babeljs.io/docs/editors/) för att konfigurera syntaxmarkering för din textredigerare.

</details>

### Hjälp, jag är fast! {#help-im-stuck}

Om du fastnar, kolla in [de tillgängliga support-resurserna](/community/support.html). I synnerhet så är [Reactiflux-chatten](https://discord.gg/0ZcbPKXt5bZjGY5n) en jättebra plats att få hjälp snabbt. Om du inte får ett svar, eller om du förblir fast, skapa en issue så hjälper vi dig.

## Överblick {#overview}

Nu när allt är uppsatt, lås oss skaffa oss en överblick av React!

### Vad är React? {#what-is-react}

React är ett deklarativt, effektivt och flexibelt JavaScript-bibliotek för att bygga användargränssnitt. Det låter dig komponera ihop komplexa gränssnitt med hjälp av mindre och isolerade delar av kod som kallas "komponenter".

React har några olika typer av komponenter, men vi börjar med typer som är subklasser av `React.Component`:

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

Vi kommer att komma till de lite konstiga XML-liknande taggarna inom kort. Vi använder komponenter för att berätta för React vad vi vill se för något på skärmen. När vår data förändras så kommer React på ett effektivt sätt att uppdatera och rita om våra komponenter.

I detta exepmel så är ShoppingList en **React-komponent-klass** eller **React-komponent-typ**. En komponent kan ta emot parametrar, så kallade `props` (förkorning av engelskans "properties") och returnerar en hierarki av vyer att visa via `render`-metoden.

`render`-metoden returnerar en *beskrivning* av vad du vill se på skärmen. React tar beskrivningen och visar resultatet. Specifikt så returnerar `render` ett **React-element**, vilket är en lättviktigt beskrivning av vad som ska ritas ut. De flesta React-utvecklare använder en speciell syntax som kallas "JSX" och som gör det enklare att skriva dessa strukturer. Koden `<div />` transformeras vid bygge till `React.createElement('div')`. Exemplet ovan är samma sak som:

```javascript
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... barn till h1 ... */),
  React.createElement('ul', /* ... barn till ul ... */)
);
```

[Se full version.](babel://tutorial-expanded-version)

Om du är intresserad så finns `createElement()` beskrivet i mer detalj i [API-dokumentationen](/docs/react-api.html#createelement), vi kommer dock inte använda den mer i denna guide. Istället kommer vi fortsätta använda oss av JSX.

JSX kommer med all funktionalitet som finns i JavaScript. Du skriva *vilket JavaScript-uttryck som helst* innanför krokiga paranteser i JSX. Varje React-element är ett JavaScript-objekt som du kan spara i en variabel och passa runt i ditt program.

`ShopplingList`-komponenten ovan rendrerar bara ut inbyggda DOM-komponenter som `<div />` och `<li />`. Men man kan komponera och rendrera egna React-komponenter också. Till exempel så kan vi nu referera till hela shoppinglistan genom att skriva `<ShoppingList />`. Varje React-komponent är inkapslad och kan fungera oberoende av andra, detta gör att du kan bygga komplexa användargränssnit från enkla komponenter.

## Inspekterande av Start-koden {#inspecting-the-starter-code}

Om du kommer arbeta med denna guide **i din webbläsrare,** öppna denna kod i en ny flik: **[Start-kod](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)**. Om du kommer arbeta med denna guide **lokalt,** öppna istället `src/index.js` i projekt-katalogen (du har redan rört denna fil under [uppsättningen](#setup-option-2-local-development-environment)).

Denna Start-kod utgör basen för det vi ska bygga. Vi har tillhandahållit all CSS-styling så att du endast behöver fokusera på att lära dig React och programmera tre-i-rad-spelet.

Genom att inspektera koden så kommer du att se att vi har tre stycken React-komponenter:

* Square
* Board
* Game

Square-komponenten rendrerar en enda `<button>` och Board-komponenten rendrerar nio Square-komponenter. Game-komponenten rendrerar en spelplan med platshållarvärden som vi kommer modifiera senare. Det finns för nävarande inga interaktiva komponenter.

### Passa data genom props {#passing-data-through-props}

För att bli varma i kläderna, låt oss prova att passa viss data från vår Board-komponent till vår Square-komponent.

Vi rekommenderar starkt att skriva kod för hand medan du arbetar dig igenom guiden och inte använda dig av kopiera/klistra in. Det kommer hjälpa dig utveckla muskelminne och bättre förståelse.

I Boards `renderSquare`-metod, ändra koden så att den passar en prop vid namn `value` till Square.

```js{3}
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;
  }
```

Ändra Squares `render`-metod så att den visar det givna värdet genom att ersätta `{/* TODO */} med `{this.props.value}`:

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

Som ett nästa steg så vill vi att Square-komponenten ska "komma ihåg" att den blivit klickad på och fylla den med en "X"-markering. För att "komma ihåg" saker använder komponenter sig av **state**.

React-komponenter kan spara state genom att sätta `this.state` i sin konstruktor. `this.state` ska anses vara privat till den React-komponent som definierat den. Låt oss spara det nuvarande värdet för Square i `this.state` och ändra det när Square blir klickad.

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

Nu ändrar vi Squares `render`-metod till att visa nuvarande värdet i state när
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

När du anropar `setState` i en komponent så kommer React automatiskt att uppdatera alla barn-komponenter inuti den också.

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

Man kan tro att Board bara borde fråga varje Square för dess state. Trots att detta tillvägagångssätt är möjligt i React så avråder vi från det för att koden blir svår att förstå, mottaglig för buggar och svår att refaktorera. I stället är det bästa tillvägagånssättet att spara spelets state i föräldra-komponenten Board till skillnad från i varje individuell Squre. Board-komponenten kan tala om för varje Square vad som ska visas genom att passa en prop [precis som vi gjorde när vi passade en siffra till varje Square](#passing-data-through-props)

**För att samla data från flera barn, eller ha två barn-komponenter som kommunicerar med varandra, behöver du deklarera det delade statet i föräldra-komponent i stället. Föräldra-komponenten kan passa statet ner till barnen genom att använda props; detta håller barn-komponenter i synk med varandra och med föräldra-komponenten.**

Lyfta state till en föräldra-komponent är vanligt när React-komponenter blir refaktorerade -- låt oss ta tillfället i akt att testa det.

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

I början så  [passade vi `value`-propen neråt](#passing-data-through-props) från Board för att visa siffror från 0 till 8 i varje Square. I ett annat, tidigare steg, ersatte vi siffrorna med en "X"-markering [bestämt av Squares egna state](#making-an-interactive-component). Det är därför som Square för närvarande ignorerar `value`-propen som passas till den av Board.

Vi kommer nu använda mekanismen att passa props igen. Vi kommer modifiera Board till att instruera varje individuell Square om dess nuvarande värde (`'X'`, `'O'`, eller `null`). Vi har redan definierat `squares`-arrayen i Boards kontruktor och vi kommer modifiera Boards `renderSquare`-metod till att läsa från det:

```javascript{2}
  renderSquare(i) {
    return <Square value={this.state.squares[i]} />;
  }
```

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/gWWQPY?editors=0010)**

Varje Square kommer nu att ta emot en `value`-prop som kommer vara antingen `'X'`, `'O'`, eller `null` för tomma rutor.

Härnäst behöver vi ändra vad som händer när en Square blir klickad. Board-komponenten håller nu reda på vilka rutor som är fyllda. Vi behöver skapa ett sätt för Square att uppdatera Boards state. Eftersom state ska anses vara privat till komponenten som definierar det, så kan vi inte uppdatera Boards state direkt från Square.

Istället passar vi ner en funktion från Board till Square och får Square att anropa på den funktionen när Square blir klickad. Vi kommer ändra `renderSquare`-metoden i Board också:

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
>Vi delar upp det returnerade elementet i flera rader för ökad läsbarhet och lade till paranteser så att inte JavaScript lägger in semikolon efter `return` och har sönder vår kod.

Nu passar vi ner två props från Board till Square: `value` och `onClick`. `onClick`-propen är en funktion som Square kan anropa när den blir klickad. Vi gör följande förändringar till Square:

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
> DOM `<button>`-elementets `onClick` attribute har speciell innebörd för React eftersom det är en inbyggd komponent. För egna komponenter som Square så är namngivningen upp till dig. Vi skulle kunna ge vilket namn som helst till Squares `onClick`-prop eller Boards `handleClick`-metod, och koden skulle fungera likadant. I React är det vedertaget att använda namn som `on[Event]` för props som representerar event och `handle[Event]` för metoder som hanterar eventen.

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

Efter dessa förändringar är vi tillbaka till att kunna klicka på Squares för att fylla dem, precis som vi kunde innan. Dock så är state nu sparat i Board-komponenten istället för i varje individuell Square-komponent. När Boards state förändras rendreras Square-komponenterna om automatiskt. Genom att hålla state av alla rutor i Board-komponenten så möjliggör det Board att avgöra vinnaren i framtiden.

Eftersom Square-komponenterna inte längre håller state så får de värden från Board-komponenten och informerar Board-komponenten när de blivit klickade. I React-termer är Square-komponenterna nu **kontrollerade komponenter**. Board har full kontroll över dem.

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

Oföränderlighet gör komplexa funktioner mycket enklare att implementera. Senare i denna guide kommer vi implementera en "time travel"-funktion som kommer tillåta oss att gå igenom tre-i-rad-spelets historik och "hoppa tillbaka" till ett föregående drag. Denna funktionalitet är inte specifik för spel -- förmågan att ångra och göra om vissa åtgärder är ett vanligt krav i applikationer. Genom att undvika direkt mutation av data så låter det oss behålla föregående version av spelets historik intakt och återanvända de i ett senare skede.

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

Varje gång en spelare gör ett drag kommer `xIsNext` (en boolesk datatyp) att växlas för att avgöra vilken spelares tur det är här näst samt spara spelets state. Vi uppdaterar Boards `handleClick`-funktion till att växla värdet av `xIsNext`:

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

När vi nu visar vilken spelares tur det är här näst borde vi också visa när spelet är vunnet och det inte längre finns några fler drag att göra. Kopiera denna hjälp-funktion och klistra in den i slutet av filen:

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

Vi kommer kalla `calculateWinner(squares)` i Boards `render`-funktion för att kolla om en spelare har vunnit. Om en spelare har vunnit kan vi visa en text såsom "Vinnare: X" eller "Vinnare: O". Vi kommer ersätta deklarationen av `status` i Boards `render`-funktion med denna kod:

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

Grattis! Du har nu ett fungerande tre-i-rad-spel. Du har också lärt dig grunderna i React. Så *du* är troligen den riktiga vinnaren här.

## Lägga till Time Travel {#adding-time-travel}

Som en sista övning, lås oss göra det möjligt att "gå tillbaka i tiden" till ett tidigare drag i spelet, kallat "time travel".

### Spara historik över dragen  {#storing-a-history-of-moves}

Om vi muterat `squares`-arrayen hade det varit väldigt svårt att implementera "time travel".

Dock så använde vi `slice()` för att skapa en kopia av `squares`-arrayen efter varhe drag och [behandlade det som oföränderligt](#why-immutability-is-important). Detta låter oss spara varje tidigare version av `squares`-arrayen och navigera mellan dragen som redan har skett.

Vi kommer spara tidigare `squares`-arrayer i en annan array kallad `history`. `history`-arrayen representerar all spelplanens state, från första till sista drag, och har en from som denna:

```javascript
history = [
  // Före första draget
  {
    squares: [
      null, null, null,
      null, null, null,
      null, null, null,
    ]
  },
  // Efter första draget
  {
    squares: [
      null, null, null,
      null, 'X', null,
      null, null, null,
    ]
  },
  // Efter andra draget
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

Nu behöver vi avgöra vilken kopmonent som borde äga `history` statet.

### Lyfta upp state, igen {#lifting-state-up-again}

Vi vill att Game-komponenten på högsta nivå ska visa en lista av tidigare drag. För att kunna göra det behöver den tillgång till `history`, därför placerar vi det statet i Game-komponenten på högsta nivå.

Genom att placera `history` statet in i Game-komponenten låter det oss ta bort `squares` state från dess barn-komponent Board. Precis som vi ["lyfte state uppåt"](#lifting-state-up) från Square-komponenten in till Board-komponentent, så lyfter vi nu det uppåt från Board-komponenten in till Game-komponenten på högsta nivån. Detta ger Game-komponentent full kontroll över Board-komponentens data och låter den instruera Board att rendrera tidigare drag från `history`.

Först så sätter vi upp det initiala statet för Game-komponentent inuti dess konstruktor:

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

Sen gör vi så att Board-komponenten tar emot `squares`- och `onClick`-props från Game-komponenten. Eftersom vi nu har en enda klickhanterare i Board som hanterar klick för flera Squares, så behöver vi passa platsen för varje Square in till `onClick`-hanteraren för att indikera vilken Square som blev klickad. Här är de nödvändiga stegen för att transformera Board-komponenten.

* Ta bort `constructor` i Board.
* Ersätt `this.state.squares[i]` med `this.props.squares[i]` i Boards `renderSquare`.
* Ersätt `this.handleClick(i)` med `this.props.onClick(i)` i Board `renderSquare`.

Board-komponenten ser nu ut så här:

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
      status = 'Vinnare: ' + winner;
    } else {
      status = 'Nästa spelare: ' + (this.state.xIsNext ? 'X' : 'O');
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

Vi uppdaterar Game-komponentens `render`-funktion till att använda det senaste draget i historiken för att avgöra och visa spelets status:

```javascript{2-11,16-19,22}
  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);

    let status;
    if (winner) {
      status = 'Vinnare: ' + winner;
    } else {
      status = 'Nästa spelare: ' + (this.state.xIsNext ? 'X' : 'O');
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

Eftersom Game-komponenten nu rendrerar spelets status kan vi ta bort motsvarande kod från Boards `render`-metod. Efter refaktorering ser Boards `render`-funktion ut så här:

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

Till sist, behöver vi flytta `handleClick`-metoden från Board-komponenten till Game-komponenten. Vi behöver också modifiera `handleClick` eftersom Game-komponentens state är lite annorlunda strukturerad. I Games `handleClick`-metod sammanfogar vi nya historik-poster in i `history`.

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

>OBS
>
>Till skillnad från array-metoden `push()`, som du kanske är mer bekant med, så muterar inte `concat()`-metoden original-arrayen, därför föredrar vi den.

Vid det här laget behöver Board-komponenten bara `renderSquare`- och `render`-metoderna. Spelets state och `handleClick`-metoden borde vara i Game-komponenten.

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/EmmOqJ?editors=0010)**

### Visa de tidigare dragen {#showing-the-past-moves}

Eftersom vi sparar tre-i-rad-spelets historik kan vi nu visa den för spelaren som en lista av tidigare drag.

Tidigare lärde vi oss att React-element är förstklassiga JavaScript-objekt, vi kan alltså passa runt dem i vår applikation. För att rendrera flera objekt i React kan vi använda en array av React-element.

I JavaScript har arrayen en [`map()`-metod](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) som vanligen används för att översätta data till annan data, till exempel:

```js
const numbers = [1, 2, 3];
const doubled = numbers.map(x => x * 2); // [2, 4, 6]
```

Genom att använda `map`-metoden kan vi översätta vår spelhistorik till React-element som representerar knappar på skärmen och visa en lista av knappar som "hoppar" till tidigare drag.

Låt oss använda `map` för att översätta `history` i Games `render`-metod:

```javascript{6-15,34}
  render() {
    const history = this.state.history;
    const current = history[history.length - 1];
    const winner = calculateWinner(current.squares);

    const moves = history.map((step, move) => {
      const desc = move ?
        'Gå till drag #' + move :
        'Gå till spelstart';
      return (
        <li>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });

    let status;
    if (winner) {
      status = 'Vinnare: ' + winner;
    } else {
      status = 'Nästa spelare: ' + (this.state.xIsNext ? 'X' : 'O');
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

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/EmmGEa?editors=0010)**

För varje drag i tre-i-rad-spelets historik skapar vi ett listobjekt `<li>` som innehåller en knapp `<button>`. Knappen har en `onClick`-hanterare som anropar en metod kallad `this.jumpTo()`. Vi har inte implementerat `jumpTo()`-metoden än. Tills vidare borde vi se en lista med drag som har skett i spelet samt en varning i utvecklarverktygens konsol som säger:

>  Warning:
>  Each child in an array or iterator should have a unique "key" prop. Check the render method of "Game".

Låt oss diskutera vad ovanstående varning betyder.

### Välja en key {#picking-a-key}

När vi rendrerar en lista sparar React lite information om varje rendrerat listobjekt. När vi uppdaterar en lista behöver React avgöra vad som har ändrats. Vi skulle kunna ha lagt till, tagit bort, ordnat om eller uppdaterat listans listobjekt.

Föreställ dig att gå från

```html
<li>Alexa: 7 uppgifter kvar</li>
<li>Ben: 5 uppgifter kvar</li>
```

till

```html
<li>Ben: 9 uppgifter kvar</li>
<li>Claudia: 8 uppgifter kvar</li>
<li>Alexa: 5 uppgifter kvar</li>
```

Utöver uppdaterade räknare skulle en människa som läser detta antagligen säga att vi bytte plats på Alexa och Bens ordning och lade till Claudia mellan Alexa och Ben. Dock så är React ett datorprogram och vet inte vad det är vi avsåg. Eftersom React inte kan veta våra intentioner behöver vi specificera en *key*-prop för varje listobjekt för att kunna skilja listobjekten åt. Ett alternativ vore att använda strängarna `alexa`, `ben`, `claudia`. Om vi visade data från en databas skulle Alexa, Ben och Claudias databas-id kunna användas som keys.

```html
<li key={user.id}>{user.name}: {user.taskCount} uppgifter kvar</li>
```

När en lista rendreras om tar React varje listobjekts key och letar i förra listan efter listobjekt som har en matchande key. Om nuvarande listan har en key som inte existerat tidigare skapar React en komponent. Om nuvarande listan saknar en key som existerade i förra listan river React ner den förra komponenten. Om två keys matchar flyttas motsvarande komponent. Keys berättar om identitet för React vilket gör att React har möjlighet att hålla reda på state mellen rendreringar. Om en komponents key ändras kommer komponenten rivas ner och återskapas med nytt state.

`key` är en speciell och reserverad prop i React (tillsammans med `ref`, en mer avancerad funktion). När ett element skapas extraherar React `key`-propen och sparar värdet direkt på det returnerade elementet. Fastän det ser ut som `key` är en del av `props` så kan den inte bli refererad till genom att använda sig av `this.props.key`. React använder automatiskt `key` för att avgöra vilken komponent som behöver uppdateras. En komponent kan inte fråga om sin `key`.

**Det är starkt rekommenderat att du tilldelar ordentliga keys närhelst du bygger dynamiska listor.** Om du inte har en ordentlig key kanske du vill överväga att strukturera om din data så att du får det.

Om ingen key är specificerad kommer React att visa en varning och använda array-indexet som key som standard. Användningen av array-indexet som key är problematiskt när man försöker ordna om eller lägga till/ta bort listobjekt. Att explicit passa `key={i}` tystar varningen med har samma problem som array-index och är inte rekommenderat i de flesta fallen.

Keys behöver inte vara globalt unika utan endast mellan komponenter och dess syskon.


### Implementera "time travel" {#implementing-time-travel}

I tre-i-rad-spelets historik har varje tidigare drag ett unikt ID associerat med sig: det är den sekventiella siffra av draget. Dragen ordnas aldrig om, tas aldrig bort eller läggs till i mitten, så det är säkert att använda drages index i arrayen som key.

I Game-komponentens `render`-metod kan vi lägga till key som `<li key={move}>` och Reacts varning om keys bordes försvinna:

```js{6}
    const moves = history.map((step, move) => {
      const desc = move ?
        'Gå till drag #' + move :
        'Gå till spelstart';
      return (
        <li key={move}>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });
```

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/PmmXRE?editors=0010)**

Klick på någon av listobjektens knappar kastar ett fel eftersom `jumpTo`-metoden är odefinierad. Innan vi implementerar `jumpTo` lägger vi till `stepNumber` till Game-komponentens state för att indikera vilket steg vi för närvarande tittar på.

Lägg först till `stepNumber: 0` till det initiala statet i Games `constructor`:

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

Sen definierar vi `jumpTo`-metoden i Game för att uppdatera `stepNumber`. Vi sätter också `xIsNext` till sant om siffran vi ändrar `stepNumber` till är jämn:

```javascript{5-10}
  handleClick(i) {
    // denna metod har inte ändrats
  }

  jumpTo(step) {
    this.setState({
      stepNumber: step,
      xIsNext: (step % 2) === 0,
    });
  }

  render() {
    // denna metod har inte ändrats
  }
```

Vi kommer nu göra några få ändringar till Games `handleClick`-metod som körs varje gång du klickar på en ruta.

`stepNumber` statet vi har lagt till återspeglar draget som visas för användaren just nu. Efter vi gjort ett drag behöver vi uppdatera `stepNumber` genom att lägga till `stepNumber: history.length` som en del av argumenten till `this.setState`. Detta säkerställer att vi inte fastnar och visar samma drag efter att ett nytt har gjorts.

Vi kommer också att ersätta läsningen från `this.state.history` med `this.state.history.slice(0, this.state.stepNumber + 1)`. Detta säkerställer att om vi "går tillbaka i tiden" och sen gör ett nytt drag från den tidpunkten, så förkastar vi alla "framtida" drag i historiken som nu skulle bli felaktiga.

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

Till sist ändrar vi Game-komponentens `render`-metod från att alltid rendrera senaste draget till att rendrera det aktuella valda draget enligt `stepNumber`:

```javascript{3}
  render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);

    // resten har inte ändrats
```

Om vi klickar på vilket drag som helst i spelets historik borde spelplanen för tre-i-rad-spelet genast uppdatera och visa hur spelplanen såg ut efter att det draget gjorts.

**[Se full kod vid denna tidpunkt](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)**

### Avslutning {#wrapping-up}

Gratulerar! Du har skapat ett tre-i-rad-spel som:

* Låter dig spela tre-i-rad,
* Visar när en spelarw har vunnit spelet,
* Sparar ett spels historik under spelets gång,
* Låter spelare återblicka ett spels historik och se tidigare versioner av spelets spelplan.

Bra jobbat! Vi hoppas att du nu känner att du har ett någorlunda grepp om hur React fungerar.

Kolla in det slutliga resultatet här: **[Slutliga resultatet](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)**.

Om du har mer tid eller vill öva dina nya färdigheter i React så kommer här några idéer på förbättringar som du skulle kunna göra till tre-i-rad-spelet, listade i ordning med ökande svårighetsgrad:

1. Visa platsen för varje drag i formatet (kolumn, rad) i listan för historiken över dragen.
2. Fetstila det aktuella valda listobjektet i listan över dragen.
3. Skriv om Board att använda två loopar för att skapa rutorna istället för att hårdkoda dem.
4. Lägg till en växlingsknapp som låter dig sortera dragen i antingen stigande eller fallande ordning.
5. När någon vinner markera de tre rutorna som utgjorde vinsten.
6. När ingen vinner, visa ett meddelande om att resultatet blev oavgjort.


Genomgående i denna guide har vi berört React-koncept såsom element, komponenter, props och state. För en mer detaljerad förklaring av vart och ett av dessa ämnen, kolla in [resten av dokumentationen](/docs/hello-world.html). För att lära dig mer om att definiera komponenter, kolla in [`React.Component` API-referens](/docs/react-component.html).
