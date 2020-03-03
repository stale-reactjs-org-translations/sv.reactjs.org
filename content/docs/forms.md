---
id: forms
title: Formulär
permalink: docs/forms.html
prev: lists-and-keys.html
next: lifting-state-up.html
redirect_from:
  - "tips/controlled-input-null-value.html"
  - "docs/forms-zh-CN.html"
---

Formelement fungerar något annorlunda än andra DOM-element i React eftersom formelement naturligt bevakar en viss intern state. Till exempel i vanlig HTML skriva så accepterar detta formulär ett enda namn:

```html
<form>
  <label>
    Namn:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

Detta formuläret har standardbeteendet för HTML-formelement att öppna en ny webbsida när det skickas. Om du vill ha detta beteende fungerar det automatiskt i React. Normalt sett är det dock föredraget att använda en JavaScript-funktion som hanterar skickandet av formuläret och har tillgång till formulärets innehåll som användaren skrivit in. Det vanligaste sättet att uppnå detta är med en teknik som kallas "kontrollerade komponenter".

## Kontrollerade komponenter {#controlled-components}

I vanliga fall bevakar HTML-formulärkontroller som `<input>`, `<textarea>`, och `<select>` deras egna state som uppdateras vid användarinmatning. I React behålls föränderlig state vanligtvis i komponenternas state-attribut och uppdateras endast via [`setState()`](/docs/react-component.html#setstate).

Vi kan kombinera dessa två genom att göra Reacts state till den "enda källan till sanningen". Då kontrollerar React-komponenten som renderar formuläret också vad som händer i detta formulär under fortsatta användarinmatningar. Ett formulär vars innehåll styrs av React på detta sätt kallas en "kontrollerad komponent".

Om vi till exempel vill göra så att det tidigare exemplet loggar namnet när det skickas kan vi skriva formuläret som en kontrollerad komponent:

```javascript{4,10-12,24}
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Ett namn skickades: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Namn:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[**Prova i CodePen**](https://codepen.io/gaearon/pen/VmmPgp?editors=0010)

Eftersom `value`-attributen är inställd på vårt HTML-formelement kommer det renderade värdet alltid att vara `this.state.value` och utgöra Reacts state till den "enda källan till sanningen". Eftersom `handleChange` anropas på varje tangenttryckning för att uppdatera Reacts state, kommer det värdet som visas att uppdateras så fort användaren gör en input.

Med en kontrollerad komponent har varje state-mutation en tillhörande hanteringsfunktion. Detta gör det enkelt att ändra eller validera användarinmatningen. Om vi till exempel vill genomföra att ett namn skrivs med veraler, kan vi skriva `handleChange` som:

```javascript{2}
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()});
}
```

## Textarea-taggen {#the-textarea-tag}

I HTML anges en `<textarea>` sitt textinnehåll av sina barn:

```html
<textarea>
  Hej, det här är lite text i ett textområde
</textarea>
```

I React används istället en `value`-attribut för en `<textarea>`. På detta sätt kan ett formulär med en `<textarea>` skrivas på samma sätt som ett formulär som använder en input med en enda rad:

```javascript{4-6,12-14,26}
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Skriv en uppsats om ditt favorit DOM-element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('En uppsats skickades: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Uppsats:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

Notera att `this.state.value` initialiseras i konstruktorn så att textfältet börjar med textinnehåll.

## Select-taggen {#the-select-tag}

I HTML skapar `<select>` en dropdownlista. Som exempel skapar den här HTML-koden en drop-downmeny med smaker:

```html
<select>
  <option value="grapefruit">Grapefrukt</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Kokosnöt</option>
  <option value="mango">Mango</option>
</select>
```

Notera att kokosnötsalternativet väljs initialt på grund av `selected`-attributet. Istället för att använda detta `selected`-attribut använder React ett `value`-attribut på det yttre `select`-taggen. Detta är mer lätthanterligt i en kontrollerad komponent, eftersom att en uppdatering endast behöver ske på ett ställe. Till exempel:

```javascript{4,10-12,24}
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Din favorit smak är: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Välj din favorit smak:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefrukt</option>
            <option value="lime">Lime</option>
            <option value="coconut">Kokosnöt</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[**Prova i CodePen**](https://codepen.io/gaearon/pen/JbbEzX?editors=0010)

Detta betyder att `<input type="text">`, `<textarea>` och `<select>` fungerar på liknande sätt - alla tar emot en `value`-attribut som kan användas för att implementera en kontrollerad komponent. 

> Obs
>
> Om du anger en array som `value`-attributet på en `select`-tagg kan du välja mer än ett alternativ:
>
>```js
><select multiple={true} value={['B', 'C']}>
>```

## Fil-input-taggen {#the-file-input-tag}

En `<input type="file">` i HTML låter användaren välja en eller flera filer från sin enhetslagring för att kunna skickas till en server eller manipuleras med JavaScript via webbläsarens [File API](https://developer.mozilla.org/en-US/docs/Web/API/File/Using_files_from_web_applications).

```html
<input type="file" />
```

Detta är en okontrollerad komponent i React eftersom dess `value` är skrivskyddat. Detta, tillsammans med andra okontrollerade komponenter, diskuteras [senare i dokumentationen](/docs/uncontrolled-components.html#the-file-input-tag).

## Hantera flera textfält {#handling-multiple-inputs}

När du behöver hantera flera kontrollerade `input`-element kan du lägga till en `name`-attribut för varje element och låta hanteringsfunktionen ta hand om vad som bör göras baserat på värdet av `event.target.name`.

Till exempel:

```javascript{15,18,28,37}
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Kommer:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Antal gäster:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

[**Prova i CodePen**](https://codepen.io/gaearon/pen/wgedvV?editors=0010)

Lägg märke till hur vi använde ES6 ["computed property name"](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names) syntaxen för att uppdatera egenskapen som motsvarar elementets `name`-attribut:

```js{2}
this.setState({
  [name]: value
});
```

Det motsvarar denna ES5-kod:

```js{2}
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```

Eftersom `setState()` automatiskt [förenar en partiell `state` och det befintlig `state`](/docs/state-and-lifecycle.html#state-updates-are-merged) behöver det enbart anropas med det `state` som har ändrats.

## Kontrollerad textfält null värde {#controlled-input-null-value}

Att specificera en `value`-attribut på en [kontrollerad komponent](/docs/forms.html#controlled-components) hindrar användaren från att ändra innehållet såvida inte detta avses. Om ett HTML-input-element fortfarande kan ändras efter du har angett en `value` är det möjligt att dess `value` är `undefined` eller `null`.

Följande kod demonstrerar detta. (`input`-elementet är först låst men kan ändras efter en kort fördröjning)

```javascript
ReactDOM.render(<input value="hi" />, mountNode);

setTimeout(function() {
  ReactDOM.render(<input value={null} />, mountNode);
}, 1000);

```

## Alternativ till kontrollerade komponenter {#alternatives-to-controlled-components}

Det kan ibland vara jobbigt att använda kontrollerade komponenter eftersom du alltid behöver skriva en eventhanterare för varje sätt som formulärinnehållet kan förändras och hanteras genom en React-komponent. Särskilt irriterande är det vid konvertering av en existerande kodbas till React eller vid integrering av en Reactapplikation med kod som inte är skriven i React. I dessa lägen kan det vara passande att kolla upp [okontrollerade komponenter](/docs/uncontrolled-components.html), en alternativ teknik för att implementera formulär.

## Kompletta lösningar {#fully-fledged-solutions}

Om du söker en komplett lösning som innehåller validering, håller koll på de besökta fälten samt hanterar hur formuläret skickas, är [Formik](https://jaredpalmer.com/formik) ett av de mest populära valen. Formik bygger på samma principer som kontrollerade komponenter och state, så negligera inte att lära dig dem.
