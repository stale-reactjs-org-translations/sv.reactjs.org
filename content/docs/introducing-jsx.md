---
id: introducing-jsx
title: Introduktion av JSX
permalink: docs/introducing-jsx.html
prev: hello-world.html
next: rendering-elements.html
---

Betrakta denna variabeldeklaration:

```js
const element = <h1>Hej, världen!</h1>;
```

Denna märkliga tagg-syntax är varken en sträng eller HTML.

Det kallas JSX, och är en syntaktisk förlängning av JavaScript. Vi rekommenderar att använda det tillsammans med React för att beskriva hur användargränssnittet ska se ut. JSX må påminna om ett template-språk, men det kommer med den fulla kraften av JavaScript.

JSX skapar React-"element". Vi kommer utforska rendering av dessa till DOM:en i [nästa sektion](/docs/rendering-elements.html). Nedan kan du finna grunderna i JSX som är nödvändiga för att komma igång.

### Varför JSX? {#why-jsx}

React omfamnar det faktum att renderingslogik är automatiskt kopplad till annan gränssnittslogik: hur händelser hanteras, hur tillstånd förändras över tid samt hur data förbereds för uppvisning.

Istället för att artificiellt separera *teknologier* genom att sätta struktur och logik i separata filer, [inkapslar](https://sv.wikipedia.org/wiki/Inkapsling_(Separation_of_Concerns)) React löst sammanhängande enheter kallade "komponenter" som innehåller både och. Vi återkommer till komponenter i ett [senare avsnitt](/docs/components-and-props.html), men om du fortfarande inte känner dig bekväm med att placera HTML i JS kanske [denna presentation](https://www.youtube.com/watch?v=x7cQ3mrcKaY) ändra din åsikt.

React [kräver inte](/docs/react-without-jsx.html) att du använder JSX, men de flesta känner att det bidrar med visuell hjälp när en arbetar med användargränssnitt i sin JavaScript-kod. Det tillåter även React att visa mer användbara fel- och varningsmeddelanden.

Med det ur vägen, låt oss börja!

### Bädda in uttryck i JSX {#embedding-expressions-in-jsx}

I exemplet nedan deklarerar vi variabeln `name` och använder den sedan i JSX genom att omsluta den i krokiga parenteser:

```js{1,2}
const name = 'Josh Perez';
const element = <h1>Hej, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

Du kan lägga vilket giltigt [JavaScript-uttryck](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) som helst inuti dessa krokiga parenteser i JSX. Till exempel så är `2 + 2`, `user.firstName`, och `formatName(user)` alla giltiga JavaScript-uttryck.

I exemplet nedan bäddar vi in resultatet av att anropa en JavaScript-funktion, `formatName(user)`, till ett `<h1>` element.

```js{12}
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hej, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[](codepen://introducing-jsx)

Vi delar upp JSX över flera rader för läsbarhetens skull. Även fast det inte är något krav, rekommenderar vi ändå att omringa det i parenteser för att undvika fallgropen som är [automatisk insättning av semikolon](https://stackoverflow.com/q/2846283).

### JSX är också ett uttryck {#jsx-is-an-expression-too}

Efter kompilering blir JSX-uttryck vanliga funktionsanrop i JavaScript och utvärderas sedan till JavaScript-objekt.

Detta betyder att du kan använda JSX inuti `if`-termer och `for`-loopar, tilldela det till variabler, mottaga som argument samt skicka tillbaka det från funktioner:

```js{3,5}
function getGreeting(user) {
  if (user) {
    return <h1>Hej, {formatName(user)}!</h1>;
  }
  return <h1>Hej, Främling.</h1>;
}
```

### Specificera Attribut med JSX {#specifying-attributes-with-jsx}

Du kan använda citationstecken för att specificera exakta strängar som attribut:

```js
const element = <div tabIndex="0"></div>;
```

Du kanske även vill använda krokiga parenteser till att bädda in JavaScript-uttryck i ett attribut:

```js
const element = <img src={user.avatarUrl}></img>;
```

Lägg inte citationstecken runt krokiga parenteser när du bäddar in ett JavaScript-uttryck i ett attribut. Istället borde du använda citationstecken (för strängvärden) eller krokiga parenteser (för uttryck), men inte i samma attribut.

>**Varning:**
>
>Eftersom JSX är närmre JavaScript än HTML, använder React DOM `camelCase` som namnkonvention till attribut istället för HTML-namnattribut.
>
>Till exempel blir `class` [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) i JSX och `tabindex` blir [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex).

### Specificera barn med JSX {#specifying-children-with-jsx}

Om en tagg är tom kan du stänga den direkt med `\>`, likt XML:

```js
const element = <img src={user.avatarUrl} />;
```

JSX-taggar kan innehålla barn:

```js
const element = (
  <div>
    <h1>Hej!</h1>
    <h2>Trevligt att träffa dig här.</h2>
  </div>
);
```

### JSX Förhindrar injektionsattacker {#jsx-prevents-injection-attacks}

Det är säkert att bädda in användarinput i JSX:

```js
const title = response.potentiallyMaliciousInput;
// Det här är säkert:
const element = <h1>{title}</h1>;
```

Per default, [undflyr](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) React DOM de värden som bäddas in i JSX innan de renderas. Därmed säkrar den att du aldrig kan injecera något som inte är explicit skrivet i din applikation. All omvandlas till en sträng innan det renderas. Detta hjälper till att förhindra attacker såsom [XSS (cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting).

### JSX Representerar Objekt {#jsx-represents-objects}

Babel kompilerar ner JSX till `React.createElement()`-anrop.

Dessa två exempel är identiska:

```js
const element = (
  <h1 className="greeting">
    Hej, världen!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hej, världen!'
);
```

`React.createElement()` utför en rad kontroller för att hjälpa dig skriva buggfri kod och till slut skapas ett objekt likt detta:

```js
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hej, världen!'
  }
};
```

Dessa objekt kallas "React element". Du kan föreställa dig dem som beskrivningar av vad du vill se på skärmen. React läser dessa objekt och använder dem för att skapa DOM:en och bibehålla den uppdaterad.

Vi kommer att utforska rendering av React-element till DOM:en i nästa avsnitt.

>**Tips:**
>
>Vi rekommenderar användningen av ["Babels" språkdefinition](https://babeljs.io/docs/editors) för ditt val av textredigerare, så att både ES6- och JSX-kod blir ordentligt belyst. Denna webbsida använder färgschemat [Oceanic Next](https://labs.voronianski.com/oceanic-next-color-scheme/) som det är kompatibelt med.
