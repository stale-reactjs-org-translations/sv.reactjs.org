---
id: rendering-elements
title: Rendering av element
permalink: docs/rendering-elements.html
redirect_from:
  - "docs/displaying-data.html"
prev: introducing-jsx.html
next: components-and-props.html
---

Element är de minsta byggstenarna i React-applikationer.

Ett element beskriver vad som skall visas på skärmen:

```js
const element = <h1>Hej, världen</h1>;
```

Till skillnad från DOM-elementen i en webbläsare är React-element enkla och kostnadseffektiva objekt. React DOM tar hand om uppdatering av DOM och tillhörande React-element.


>**Obs:**
>
>En kan förväxla element med det mer kända begreppet "komponenter". Vi kommer att introducera komponenter i [nästa avsnitt](/docs/components-and-props.html). Elementen utgör grunden för komponenterna, så vi rekommenderar dig att läsa detta avsnitt innan du går vidare.

## Rendering av element i DOM {#rendering-an-element-into-the-dom}

Låt oss säga att du har en `<div>` någonstans i din HTML-fil:

```html
<div id="root"></div>
```

Vi kallar detta en "root"-DOM-nod eftersom allt inom den kommer att hanteras av React DOM.

Applikationer byggda med bara React har vanligtvis en enda "root"-DOM-nod. Om du integrerar React i en befintlig app så kan du ha så många isolerade DOM-noder som du vill.

<<<<<<< HEAD
För att rendera ett React-element i en "root"-DOM-nod, skicka båda till `ReactDOM.render()`:
=======
To render a React element into a root DOM node, pass both to [`ReactDOM.render()`](/docs/react-dom.html#render):
>>>>>>> bd0c9d8c5f5e78e0bd1066b46024ba98f4daac84

`embed:rendering-elements/render-an-element.js`

[](codepen://rendering-elements/render-an-element)

Det visar "Hej, världen" på sidan.

## Uppdatera det renderade elementet {#updating-the-rendered-element}

React-element är [oföränderliga](https://en.wikipedia.org/wiki/Immutable_object). När du väl har skapat ett element kan du inte ändra på dess barn eller attribut. Ett element är likt en stillbild i en film: den representerar användargränssnittet vid en specfik tidpunkt.

<<<<<<< HEAD
Med vad vi har lärt oss hittills är det enda sättet att uppdatera användargränssnittet, att skapa ett nytt element och skicka det till `ReactDOM.render()`.
=======
With our knowledge so far, the only way to update the UI is to create a new element, and pass it to [`ReactDOM.render()`](/docs/react-dom.html#render).
>>>>>>> bd0c9d8c5f5e78e0bd1066b46024ba98f4daac84

Låt oss ta detta exempel på en tickande klocka:

`embed:rendering-elements/update-rendered-element.js`

[](codepen://rendering-elements/update-rendered-element)

<<<<<<< HEAD
Den kallar på `ReactDOM.render()` varje sekund från en [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) callback.
=======
It calls [`ReactDOM.render()`](/docs/react-dom.html#render) every second from a [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) callback.
>>>>>>> bd0c9d8c5f5e78e0bd1066b46024ba98f4daac84

>**Obs:**
>
<<<<<<< HEAD
>I praktiken kallar de flesta React-applikationer på `ReactDOM.render()` endast en gång. I nästa avsnitt lär vi oss hur vi kapslar in sådan kod i [komponenter med state](/docs/state-and-lifecycle.html).
=======
>In practice, most React apps only call [`ReactDOM.render()`](/docs/react-dom.html#render) once. In the next sections we will learn how such code gets encapsulated into [stateful components](/docs/state-and-lifecycle.html).
>>>>>>> bd0c9d8c5f5e78e0bd1066b46024ba98f4daac84
>
>Vi rekomenderar att du läser avsnitten i ordning då de bygger vidare på varandra.

## React uppdaterar endast det nödvändigaste {#react-only-updates-whats-necessary}

React DOM jämför det nuvarande elementet och dess barn med det föregående, och tillämpar endast de DOM-uppdateringar som innehåller en förändring.

Du kan verifiera genom att inspektera det [senaste exemplet](codepen://rendering-elements/update-rendered-element) med utvecklingsverktygen för webbläsare:


![DOM-inspekteraren som visar uppdateringar](../images/docs/granular-dom-updates.gif)

Trots att vi skapar ett element som beskriver hela användargränssnittet varje tick, kommer endast textnoden vars innehåll har ändrats att uppdateras av React DOM.

<<<<<<< HEAD
Enligt vår erfarenhet är det mer meningsfullt att tänka på hur användargränssnittet ser ut vid en given tidpunkt, snarare än hur det kommer att förändras över tiden. Detta tänkande förhindrar en hel rad misstag.
=======
In our experience, thinking about how the UI should look at any given moment, rather than how to change it over time, eliminates a whole class of bugs.
>>>>>>> bd0c9d8c5f5e78e0bd1066b46024ba98f4daac84
