---
id: components-and-props
title: Komponenter och Props
permalink: docs/components-and-props.html
redirect_from:
  - "docs/reusable-components.html"
  - "docs/reusable-components-zh-CN.html"
  - "docs/transferring-props.html"
  - "docs/transferring-props-it-IT.html"
  - "docs/transferring-props-ja-JP.html"
  - "docs/transferring-props-ko-KR.html"
  - "docs/transferring-props-zh-CN.html"
  - "tips/props-in-getInitialState-as-anti-pattern.html"
  - "tips/communicate-between-components.html"
prev: rendering-elements.html
next: state-and-lifecycle.html
---

Komponenter låter dig bryta upp användargränssnittet i oberoende, återanvändbara bitar och tänka på varje bit isolerat. Den här sidan ger dig en introduktion till tanken bakom komponenter. Du kan hitta ett [detaljerat API för komponenter här](/docs/react-component.html).

Konceptuellt är komponenter som JavaScript-funktioner. De accepterar godtyckliga inputs (kallade "props") och returnerar React-element som beskriver vad som ska visas på skärmen.

## Funktion- och klasskomponenter {#function-and-class-components}

Det enklaste sättet att definiera en komponent är att skriva en JavaScript-funktion:

```js
function Welcome(props) {
  return <h1>Hej, {props.name}</h1>;
}
```

Denna funktion är en giltig React-komponent eftersom den accepterar en enda "props" (som står för properties) objektargument med data och returnerar ett React-element. Vi kallar sådana komponenter "funktionskomponenter" eftersom de bokstavligen är JavaScript-funktioner.

Du kan även använda en [ES6-klass](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) för att definiera en komponent:

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hej, {this.props.name}</h1>;
  }
}
```

De två ovannämnda komponenterna är identiska ur Reacts synvinkel.

Klasser har några ytterligare funktioner, som vi kommer att diskutera i [senare avsnitt](/docs/state-and-lifecycle.html). Fram till dess kommer vi att använda för funktionskomponenter för deras tydlighet.

## Rendering av en komponent {#rendering-a-component}

Tidigare har vi bara stött på React-element som representerade DOM-taggar:

```js
const element = <div />;
```

Elementen kan också representera användardefinierade komponenter:

```js
const element = <Welcome name="Sara" />;
```

När React upptäcker ett element som representerar en användardefinierad komponent, överför React alla JSX attribut till denna komponent som ett enda objekt. Vi kallar detta objekt "props".

Till exempel renderar denna kod "Hej, Sara" på sidan:

```js{1,5}
function Welcome(props) {
  return <h1>Hej, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[](codepen://components-and-props/rendering-a-component)

Låt oss sammanfatta vad som händer i det här exemplet:

1. Vi kallar `ReactDOM.render()` med React-elementet `<Welcome name="Sarah" />`.
2. React anropar `Welcome` komponenten med `{name: 'Sara'}` som dess props.
3. Vår `Welcome` komponent returnerar elementet `<h1>Hej, Sara</h1>` som resultat.
4. React DOM uppdaterar effektivt DOM för att matcha `<h1>Hej, Sara</h1>`.

>**Obs:** Börja alltid komponentnamnen med en stor bokstav.
>
>React behandlar komponenter som börjar med små bokstäver som DOM-taggar. Exempelvis representerar `<div />` en HTML-div-tagg, men `<Welcome />` representerar en komponent och kräver att `Welcome` definieras.
>
>För att lära dig mer om resonemanget bakom denna konvention, läs
 [JSX i detalj](/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized).

## Komponering av komponenter {#composing-components}

Komponenter kan hänvisa till andra komponenter i deras output. Det tillåter oss att använda samma komponentabstraktion för alla detaljnivåer. Eftersom knappar, formulär och skärmar i allmänhet kallas komponenter i React-appar.

Vi kan till exempel skapa en `App`-komponent som renderar `Welcome` flera gånger:

```js{8-10}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[](codepen://components-and-props/composing-components)

Typically, new React apps have a single `App` component at the very top. However, if you integrate React into an existing app, you might start bottom-up with a small component like `Button` and gradually work your way to the top of the view hierarchy.

Generellt sett har nya React-applikationer en enda `App`-komponent i roten. Å andra sidan, om du integrerar React i en befintlig app, kan du starta nerifrån och upp, med en liten komponent som `Button` och gradvis arbeta dig till toppen av visningshierarkin.

## Extrahera komponenter {#extracting-components}

Var inte rädd för att dela upp komponenter i mindre komponenter.

Låt oss ta denna `Comment` komponent som exempel:

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[](codepen://components-and-props/extracting-components)

Den tar emot `author` (ett objekt), `text` (en sträng) och `date` (ett datum) som Props och beskriver en kommentar på en webbplats för sociala medier.

This component can be tricky to change because of all the nesting, and it is also hard to reuse individual parts of it. Let's extract a few components from it.

Denna komponent kan vara svår att ändra på grund av all nestling, det är också svårt att återanvända enskilda delar av den. Låt oss extrahera några komponenter från den.

Till att börja med så extraherar vi `Avatar`:

```js{3-6}
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

`Avatar` behöver inte veta att den renderas i en `Comment`-komponent. Därför har vi gett dess Props ett mer generiskt namn: `user` istället för `author`.

Vi rekommenderar att namnge props från komponentens egen perspektiv snarare än i det sammanhang där den används.

Vi kan nu förenkla `Comment` lite:

```js{5}
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Sedan extraherar vi en `UserInfo`-komponent som visar en `Avatar` bredvid användarens namn:

```js{3-8}
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

Detta låter oss förenkla `Comment` ytterligare:

```js{4}
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[](codepen://components-and-props/extracting-components-continued)

Till en början kan extrahering av komponenter verka tråkigt, men att ha ett antal återanvändbara komponenter lönar sig i slutändan för större applikationer. I allmänhet, om delar av ditt användargränssnitt används flera gånger (`Button`, `Panel`, `Avatar`) eller om det är tillräckligt komplex i sig själv (`App`, `FeedStory`, `Comment`), är det en bra kandidat för en återanvändbar komponent.

## Props är skrivskyddad {#props-are-read-only}

Oavsett om du deklarerar en komponent [som en funktion eller en klass](#function-and-class-components), får den aldrig ändra sina egna props. Ta denna `sum` funktion:

```js
function sum(a, b) {
  return a + b;
}
```

Sådana funktioner kallas för ["rena"](https://en.wikipedia.org/wiki/Pure_function) eftersom de inte ändrar sina inputs och alltid returnerar samma resultat för samma inputs.

Som kontrast är denna funtion inte en "ren" funktion eftersom den ändrar sin egen input:

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

React är ganska flexibel men det har en strikt regel:

**Alla React-komponenter måste bete sig som "rena" funktioner i förhållande till deras Props.**

Naturligtvis är applikationsgränssnitt dynamiska och förändras över tid. I [nästa avsnitt](/docs/state-and-lifecycle.html) kommer vi att introducera ett nytt begrepp "state". State tillåter React-komponenter att ändra sin output över tid som svar på användaråtgärder, nätverkssvar och mycket annat utan att bryta mot denna regel.
