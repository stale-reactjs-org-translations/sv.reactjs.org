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

För att rendera ett React-element i en "root"-DOM-nod, skicka båda till [`ReactDOM.render()`](/docs/react-dom.html#render):

`embed:rendering-elements/render-an-element.js`

**[Try it on CodePen](https://codepen.io/gaearon/pen/ZpvBNJ?editors=1010)**

Det visar "Hej, världen" på sidan.

## Uppdatera det renderade elementet {#updating-the-rendered-element}

React-element är [oföränderliga](https://en.wikipedia.org/wiki/Immutable_object). När du väl har skapat ett element kan du inte ändra på dess barn eller attribut. Ett element är likt en stillbild i en film: den representerar användargränssnittet vid en specfik tidpunkt.

Med vad vi har lärt oss hittills är det enda sättet att uppdatera användargränssnittet, att skapa ett nytt element och skicka det till [`ReactDOM.render()`](/docs/react-dom.html#render).

Låt oss ta detta exempel på en tickande klocka:

`embed:rendering-elements/update-rendered-element.js`

**[Try it on CodePen](https://codepen.io/gaearon/pen/gwoJZk?editors=1010)**

Den kallar på [`ReactDOM.render()`](/docs/react-dom.html#render) varje sekund från en [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) callback.

>**Obs:**
>
>I praktiken kallar de flesta React-applikationer på [`ReactDOM.render()`](/docs/react-dom.html#render) endast en gång. I nästa avsnitt lär vi oss hur vi kapslar in sådan kod i [komponenter med state](/docs/state-and-lifecycle.html).
>
>Vi rekomenderar att du läser avsnitten i ordning då de bygger vidare på varandra.

## React uppdaterar endast det nödvändigaste {#react-only-updates-whats-necessary}

React DOM jämför det nuvarande elementet och dess barn med det föregående, och tillämpar endast de DOM-uppdateringar som innehåller en förändring.

<<<<<<< HEAD
Du kan verifiera genom att inspektera det [senaste exemplet](codepen://rendering-elements/update-rendered-element) med utvecklingsverktygen för webbläsare:
=======
You can verify by inspecting the [last example](https://codepen.io/gaearon/pen/gwoJZk?editors=1010) with the browser tools:
>>>>>>> 5f0549c86e7a9c0774e66687d1bc0118a681eb9d


![DOM-inspekteraren som visar uppdateringar](../images/docs/granular-dom-updates.gif)

Trots att vi skapar ett element som beskriver hela användargränssnittet varje tick, kommer endast textnoden vars innehåll har ändrats att uppdateras av React DOM.

Enligt vår erfarenhet är det mer meningsfullt att tänka på hur användargränssnittet ser ut vid en given tidpunkt, snarare än hur det kommer att förändras över tiden. Detta tänkande förhindrar en hel rad misstag.
