---
id: handling-events
title: Hantera events
permalink: docs/handling-events.html
prev: state-and-lifecycle.html
next: conditional-rendering.html
redirect_from:
  - "docs/events-ko-KR.html"
---

Att hantera event med React-element är väldigt likt att hantera event på DOM-element. Det finns några syntaktiska skillnader:

* React-event är namngivna med camelCase, snarare än med gemener.
* Med JSX så passar du en funktion som event-hanterare, snarare än en sträng.

Till exempel, HTML-koden:

```html
<button onclick="activateLasers()">
  Aktivera lasrar
</button>
```

är något annorlunda i React:

```js{1}
<button onClick={activateLasers}>
  Aktivera lasrar
</button>
```

En annan skillnad är att du, i React, inte kan returnera `false` för att förhindra standardbeteende. Du måste explicit anropa `preventDefault`. För att till exempel förhindra standardlänkbeteendet för att öppna en ny sida, kan du i vanlig HTML skriva:

```html
<a href="#" onclick="console.log('Länken blev klickad.'); return false">
  Klicka mig
</a>
```

I React, är detta istället:

```js{2-5,8}
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('Länken blev klickad.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Klicka mig
    </a>
  );
}
```

Här är `e` ett syntetiskt event. React definierar dessa syntetiska event enligt [W3C-specen] (https://www.w3.org/TR/DOM-Level-3-Events/), så du behöver inte oroa dig för kompatibilitet mellan webbläsare. Se referenshandboken för [`SyntheticEvent`](/docs/events.html) för mer information.

När du använder React ska du i allmänhet inte behöva anropa `addEventListener` för att lägga till lyssnare till ett DOM-element efter det att det har skapats. I stället förse elementet med en lyssnare när det ursprungligen rendreras.

När du definierar en komponent med en [ES6-klass](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) är ett vanligt mönster för en event-hanterare att vara en metod i klassen. Till exempel rendrerar denna `Toggle`-komponent en knapp som låter användaren växla mellan "ON" och "OFF"-läge:

```js{6,7,10-14,18}
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // Detta bindande är nödvändingt för att få `this` att fungera i callbacken
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

[**Prova det på CodePen**](https://codepen.io/gaearon/pen/xEmzGg?editors=0010)

Du måste vara försiktig med betydelsen av `this` i JSX-callbacks. I JavaScript är klassmetoder inte [bundna](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) som standard. Om du glömmer att binda `this.handleClick` och vidarebefordra det till `onClick`, kommer `this` att vara odefinierat när funktionen faktiskt kallas.

Detta är inte React-specifikt beteende; det är en del av [hur funktioner fungerar i JavaScript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/). I allmänhet, om du hänvisar till en metod utan `()` efter den, till exempel `onClick={this.handleClick}`, bör du binda den metoden.

Om du irriterar dig på att anropa `bind`, finns det två sätt du kan komma runt detta. Om du använder den experimentella [publika klassfält-syntaxen](https://babeljs.io/docs/plugins/transform-class-properties/) kan du använda klassfält för att binda callbacks korrekt:

```js{2-6}
class LoggingButton extends React.Component {
  // Denna syntax säkerställer att `this` är bunden inuti handleClick.
  // Varning: detta är en *experimentell* syntax.
  handleClick = () => {
    console.log('this är:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Klicka mig
      </button>
    );
  }
}
```

Denna syntax är aktiverad som standard i [Create React App](https://github.com/facebookincubator/create-react-app).

Om du inte använder klassfält-syntax så kan du använda en [pil-function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) i callbacken:

```js{7-9}
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this är:', this);
  }

  render() {
    // Denna syntax säkerställer att `this` är bunden inuti handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Klicka
      </button>
    );
  }
}
```

Problemet med den här syntaxen är att en annan callback skapas varje gång `LoggingButton` rendreras. I de flesta fall är det ok. Men om denna callback skickas som en prop till barn-komponenter, kan dessa komponenter råka göra en extra rendrering. Generellt sett rekommenderar vi att binda in konstruktorn eller använda klassfält-syntax för att undvika denna typ av prestandaproblem.

## Passa argument till event-hanterare {#passing-arguments-to-event-handlers}

Inuti en loop är det vanligt att man vill passa extra parametrar till en event hanterare. Till exempel om `ìd` är radens ID skulle något av följande fungera:

```js
<button onClick={(e) => this.deleteRow(id, e)}>Ta bort rad</button>
<button onClick={this.deleteRow.bind(this, id)}>Ta bort rad</button>
```

Ovanstående två rader är likvärdiga och använder [pil-funktioner](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) och [`Function.prototype.bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind).

I båda fallen kommer `e`-argumentet som representerar React-eventet att skickas som ett andra argument efter ID. Med en pilfunktion måste vi explicit skicka den, men med `bind` vidarebefordras alla ytterligare argument automatiskt.
