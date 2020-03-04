---
id: state-and-lifecycle
title: State och livscykel
permalink: docs/state-and-lifecycle.html
redirect_from:
  - "docs/interactivity-and-dynamic-uis.html"
prev: components-and-props.html
next: handling-events.html
---

Denna sida introducerar konceptet state och livscykel i en React-komponent. Du hittar en [detaljerad API-referens här](/docs/react-component.html).

Tänk på det tickande klockexemplet från [ett av de föregående avsnitten](/docs/rendering-elements.html#updating-the-rendered-element). I [Rendering av element](/docs/rendering-elements.html#rendering-an-element-into-the-dom) har vi bara lärt oss ett sätt att uppdatera användargränssnittet. Vi kallar `ReactDOM.render()` för att ändra den renderade outputen:

```js{8-11}
function tick() {
  const element = (
    <div>
      <h1>Hej, världen!</h1>
      <h2>Klockan är {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[**Prova i CodePen**](https://codepen.io/gaearon/pen/gwoJZk?editors=0010)

I den här sektionen kommer vi att lära oss hur man gör `Clock`-komponenten verkligt återanvändbar och inkapslad. Den kommer att ställa in sin egen timer och uppdatera sig själv varje sekund.

Vi kan börja med att kapsla in hur klockan ser ut:

```js{3-6,12}
function Clock(props) {
  return (
    <div>
      <h1>Hej, världen!</h1>
      <h2>Klockan är {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[**Prova i CodePen**](https://codepen.io/gaearon/pen/dpdoYR?editors=0010)

Men den saknar ett avgörande krav: det faktum att `Clock` ställer in en timer och uppdaterar användargränssnittet varje sekund bör vara en implementeringsdetalj för `Clock`.

Helst vill vi skriva det en gång och låta `Clock` uppdatera sig själv:

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

För att implementera detta måste vi lägga till "state" till `Clock`-komponenten.

State liknar props, men den är privat och kontrolleras helt av komponenten.

## Att konvertera en funktion till en klass {#converting-a-function-to-a-class}

Du kan konvertera en funktionskomponent som `Clock` till en klass i fem steg:

1. Skapa en [ES6-klass](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes), med samma namn, som extendar `React.Component`.

2. Lägg till en enda tom metod till den som heter `render()`.

3. Flytta funktionens innehåll till `render()`-metoden.

4. Byt ut `props` mot `this.props` i `render()`-innehållet.

5. Radera den återstående tomma funktionsdeklarationen.

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hej, världen!</h1>
        <h2>Klockan är {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

[**Prova i CodePen**](https://codepen.io/gaearon/pen/zKRGpo?editors=0010)

`Clock` definieras nu som en klass istället för en funktion.

`render`-metoden kommer att anropas varje gång en uppdatering sker, men så länge vi renderar `<Clock />` i samma DOM-nod kommer bara en enda instans av klassen att användas. Detta låter oss använda ytterligare funktioner som lokal state och livscykelmetoder.

## Att lägga till lokal state i en klass {#adding-local-state-to-a-class}

Vi kommer att flytta `date` från props till state i tre steg:

1) Byt ut `this.props.date` mot `this.state.date` i `render()`-metoden:

```js{6}
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hej, världen!</h1>
        <h2>Klockan är {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

2) Lägg till en [class constructor](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor) som tilldelar den ursprungliga `this.state`:

```js{4}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hej, världen!</h1>
        <h2>Klockan är {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```


Notera hur vi skickar `props` till bas-constructorn:

```js{2}
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
```

Klasskomponenter ska alltid anropa bas-constructorn med `props`.

3) Ta bort `date`-prop från `<Clock />`-elementet:

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

Vi kommer senare att lägga till timerkoden till själva komponenten.

Resultatet ser ut så här:

```js{2-5,11,18}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hej, världen!</h1>
        <h2>Klockan är {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[**Prova i CodePen**](https://codepen.io/gaearon/pen/KgQpJd?editors=0010)

Därefter gör vi så att `Clock` ställer in sin egen timer och uppdaterar sig själv varje sekund.

## Att lägga till livscykelmetoder i en klass {#adding-lifecycle-methods-to-a-class}

I applikationer med många komponenter är det mycket viktigt att frigöra resurser som komponenterna tar när de förstörs.

Vi vill [ställa in en timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) när `Clock` renderas till DOMen för första gången. Detta kallas "mounting" i React.

Vi vill också [rensa den timern](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval) när DOMen som produceras av `Clock` tas bort. Detta kallas "unmounting" i React.

Vi kan förklara speciella metoder för komponentklassen för att köra viss kod när en komponent mountas och unmountas:

```js{7-9,11-13}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hej, världen!</h1>
        <h2>Klockan är {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Dessa metoder kallas "livscykelmetoder".

Metoden `componentDidMount()` körs efter att komponent-outputen har återgivits till DOMen. Detta är ett bra ställe att ställa in en timer:

```js{2-5}
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

Lägg märke till hur vi sparar timer-ID rätt på `this` (`this.timerID`).

Medan `this.props` är inställt av React själv och `this.state` har en speciell betydelse, är du fri att lägga till ytterligare fält till klassen manuellt om du behöver lagra något som inte deltar i dataflödet (som ett timer-ID).

Vi kommer att riva ned timern i livscykelmetoden `componentWillUnmount()`:

```js{2}
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

Slutligen kommer vi att implementera en metod som kallas `tick()` som `Clock`-komponenten kommer att köra varje sekund.

Den använder `this.setState()` för att schemalägga uppdateringar av komponentens lokala state:

```js{18-22}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hej, världen!</h1>
        <h2>Klockan är {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[**Prova i CodePen**](https://codepen.io/gaearon/pen/amqdNA?editors=0010)

Nu tickar klockan varje sekund.

Låt oss snabbt sammanfatta vad som händer och i vilken ordning metoderna kallas:

1) När `<Clock />` skickas till `ReactDOM.render()`, anropar React constructorn för `Clock`-komponenten. Eftersom `Clock` måste visa aktuell tid initialiserar den `this.state` med ett objekt som innehåller aktuell tid. Vi kommer senare att uppdatera denna state.

2) React anropar sedan `Clock`-komponentens `render()`-metod. Så här får React veta vad som ska visas på skärmen. React uppdaterar sedan DOMen för att matcha renderings-outputen av `Clock`.

3) När `Clock`-outputen sätts in i DOMen, anropar React livscykelmetoden `componentDidMount()`. Inne i den ber `Clock`-komponenten webbläsaren att ställa in en timer för att anropa komponentens `tick()`-metod en gång per sekund.

4) Varannan sekund anropar webbläsaren `tick()`-metoden. Inne i den schemalägger `Clock`-komponenten en användargränssnitts-uppdatering genom att anropa `setState()` med ett objekt som innehåller aktuell tid. Tack vare `setState()`-anropet vet React att staten har ändrats och anropar `render()`-metoden igen för att veta vad som ska visas på skärmen. Den här gången kommer `this.state.date` i `render()`-metoden att vara annorlunda, och därför kommer renderings-outputen att inkludera den uppdaterade tiden. React uppdaterar DOMen därefter.

5) Om `Clock`-komponenten någonsin tas bort från DOMen, anropar React livscykelmetoden `componentWillUnmount()` så att timern stoppas.

## Att använda state korrekt {#using-state-correctly}

Det finns tre saker du bör veta om `setState()`.

### Modifiera inte state direkt {#do-not-modify-state-directly}

Till exempel, detta kommer inte att rendera om en komponent:

```js
// Fel
this.state.comment = 'Hej';
```

Använd `setState()` istället:

```js
// Rätt
this.setState({comment: 'Hej'});
```

Den enda platsen där du kan tilldela `this.state` är constructorn.

### State-uppdateringar kan vara asynkrona {#state-updates-may-be-asynchronous}

React kan samla flera `setState()`-anrop till en enda uppdatering av prestandaskäl.

Eftersom `this.props` och `this.state` kan uppdateras asynkront bör du inte lita på deras värden för att beräkna nästa state.

Till exempel kan den här koden misslyckas med att uppdatera `Counter`-komponenten:

```js
// Fel
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

För att fixa det använder du en annan form av `setState()` som accepterar en funktion istället för ett objekt. Den funktionen kommer att få den tidigare staten som det första argumentet, och props, när uppdateringen tillämpas, som det andra argumentet:

```js
// Rätt
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

Vi använde en [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) ovan, men det fungerar också med vanliga funktioner:

```js
// Correct
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```

### State-uppdateringar slås samman {#state-updates-are-merged}

När du anropar `setState()` slår React samman objektet du tillhandahåller i den aktuella staten.

Till exempel kan din state innehålla flera oberoende variabler:

```js{4,5}
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

Sedan kan du uppdatera dem oberoende med separata `setState()`-anrop:

```js{4,10}
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

Sammanslagningen är grund, så `this.setState({comments})` lämnar `this.state.posts` intakt, men ersätter helt` this.state.comments`.

## Data flödar nedåt {#the-data-flows-down}

Varken föräldrar eller barnkomponenter kan veta om en viss komponent har state eller inte, och de borde inte bry sig om den definieras som en funktion eller en klass.

Det är därför staten ofta kallas lokal eller inkapslad. Den är inte tillgänglig för någon annan komponent än den som äger och ställer in den.

En komponent kan välja att överföra sin state som props till sina barnkomponenter:

```js
<h2>Klockan är {this.state.date.toLocaleTimeString()}.</h2>
```

Detta fungerar också för användardefinierade komponenter:

```js
<FormattedDate date={this.state.date} />
```

Komponenten `FormattedDate` skulle få `date` i sina props och skulle inte veta om det kom från state i `Clock`, från props i `Clock` eller om det skrivs för hand:

```js
function FormattedDate(props) {
  return <h2>Klockan är {props.date.toLocaleTimeString()}.</h2>;
}
```

[**Prova i CodePen**](https://codepen.io/gaearon/pen/zKRqNB?editors=0010)

Detta kallas vanligtvis ett "top-down" eller "unidirectional" dataflöde. All state ägs alltid av någon specifik komponent, och all data eller användargränssnitt som härrör från den staten kan bara påverka komponenter "under" dem i trädet.

Om du föreställer dig ett komponentträd som ett vattenfall av props, är varje komponents state som en ytterligare vattenkälla som sammanfogar det vid en godtycklig punkt men också flyter ner.

För att visa att alla komponenter verkligen är isolerade, kan vi skapa en `App`-komponent som renderar tre `<Clock>`-komponenter:

```js{4-6}
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[**Prova i CodePen**](https://codepen.io/gaearon/pen/vXdGmd?editors=0010)

Varje `Clock` sätter upp sin egen timer och uppdateras oberoende.

I React-appar anses det, oavsett om en komponent har state eller inte, som en implementeringsdetalj av komponenten som kan förändras över tid. Du kan använda komponenter utan state i komponenter med state och vice versa.
