---
id: hello-world
title: Hello World
permalink: docs/hello-world.html
prev: cdn-links.html
next: introducing-jsx.html
---

Ett litet React-exempel ser ut så här:

```js
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

Det visar rubriken "Hello, world!" på webbsidan.

[](codepen://hello-world)

Klicka på länken ovan för att öppna en kodredigerare online. Testa att göra ändringar i koden och se hur de påverkar innehållet som visas. De flesta sidor i den här guiden kommer att ha exempel som detta där du kan göra ändringar och testa dig fram.


## Hur den här guiden fungerar {#how-to-read-this-guide}

I den här guiden kommer vi att titta närmre på de byggblock som en React-app består utav: element och komponenter. När du behärskar dem kan du skapa komplexa appar utifrån mindre byggnadsdelar som kan återanvändas.

>Tips
>
>Den här guiden är skapad för de som föredrar att  **lära sig koncept steg-för-steg**. Om du föredrar att lära dig genom att göra, kolla in vår [praktiska guide](/tutorial/tutorial.html). Den här guiden, och den praktiska guiden, kan även vara bra komplement till varandra.

Det här är det första kapitlet i steg-för-steg guiden om de huvudsakliga begreppen inom React. Du hittar en lista med alla kapitel i menyn i sidspalten. Om du läser detta via mobilen hittar du menyn genom att klicka på knappen i det nedre högra hörnet på din skärm.

Varje kapitel i den här guiden bygger på kunskapen du fått i de föregående kapitlen. **Du kan lära dig det mesta om React genom att läsa kapitlen i guiden ”Huvudsakliga begrepp” i den ordning de visas i sidspalten.** Till exempel är [“Introduktion till JSX”](/docs/introducing-jsx.html) nästa kapitel efter detta.

## Antaganden om kunskapsnivå {#knowledge-level-assumptions}

React är ett JavaScript-bibliotek och därför kommer vi anta att du har en basal förståelse för JavaScript som programmeringsspråk. **Om du inte känner dig väldigt säker på din kunskapsnivå rekommenderar vi [att gå igenom en guide om JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) för att se hur du ligger till**, så att du kan följa med i guiden utan att tappa bort dig. Det kan ta mellan 30 min och en timme, men resultatet blir att du inte behöver känna det som att du lär dig både React och Javascript på samma gång.

>Notera
>
>Den här guiden använder till och från nyare JavaScript-syntax i exemplen. Om du inte har jobbat med JavaScript under de senaste åren bör [dessa tre punkter](https://gist.github.com/gaearon/683e676101005de0add59e8bb345340c) hjälpa dig på vägen.


## Nu kör vi igång! {#lets-get-started}

Fortsätt scrolla, länken till [nästa kapitel i den här guiden](/docs/introducing-jsx.html) hittar du precis innan sidfoten.
