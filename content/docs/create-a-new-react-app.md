---
id: create-a-new-react-app
title: Skapa en ny React-app
permalink: docs/create-a-new-react-app.html
redirect_from:
  - "docs/add-react-to-a-new-app.html"
prev: add-react-to-a-website.html
next: cdn-links.html
---

Använd en integrerad verktygskedja för en optimal användar- och utvecklarupplevelse.

Den här sidan beskriver några få populära React-verktygskedjor som hjälper till med saker såsom:

* Växa till många filer och komponenter.
* Använda tredjepartsbibliotek från npm.
* Upptäcka vanliga misstag tidigt.
* Redigera CSS och JS i realtid under utveckling.
* Optimera output:en för produktion.

De verktygskedjor som rekommenderas på den här sidan **kräver ingen konfiguration för att komma igång**.

## Du kanske inte behöver någon verktygskedja  {#you-might-not-need-a-toolchain}

Om du inte upplever problemen som nämndes ovan eller inte känner dig bekväm i att använda JavaScript-verktyg ännu, överväg [att lägga till React som en vanlig `<script>`-tagg på en HTML-sida](/docs/add-react-to-a-website.html), alternativt även [med JSX](/docs/add-react-to-a-website.html#optional-try-react-with-jsx).

￼Detta är också **det lättaste sättet att integrera React i en befintlig webbsida**. Du kan alltid lägga till en större verktygskedja om du tycker att det vore hjälpsamt!

## Rekommenderade verktygskedjor {#recommended-toolchains}

React-teamet rekommenderar i huvudsak dessa lösningar:

- Om du **håller på att lära dig React** eller **skapar en ny [single-page](/docs/glossary.html#single-page-application)-app**, använd [Create React App](#create-react-app).
- Om du bygger en **serverrenderad webbsida med Node.js**, prova [Next.js](#nextjs).
- Om du bygger en **statisk innehållsorienterad webbsida**, prova [Gatsby](#gatsby).
- Om du bygger ett **komponentbibliotek** eller **integrerar med en befintlig kodbas**, se [Mer flexibla verktygskedjor](#more-flexible-toolchains).

### Create React App {#create-react-app}

[Create React App](https://github.com/facebookincubator/create-react-app) är en bekväm miljö för **att lära sig React**, och är det bästa sättet att börja bygga **en ny [single-page](/docs/glossary.html#single-page-application)-applikation** i React.

Det sätter upp din utvecklingsmiljö så att du kan använda de senaste JavaScript-funktionerna, ger dig en trevlig utvecklarupplevelse, och optimerar din app för produktion. Du kommer att behöva Node >= 8.10 och npm >= 5.6 på din enhet. För att skapa ett projekt, kör:

```bash
npx create-react-app min-app
cd min-app
npm start
```

>Tänk på
>
>`npx` på den första raden är inget skrivfel — det är ett [verktyg för att köra paket som är inkluderat i npm 5.2+](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b).

Create React App hanterar inte backendlogik eller databaser; det skapar bara en byggpipeline för frontend, så du kan använda den med vilken backend du vill. Under huven används [Babel](https://babeljs.io/) och [webpack](https://webpack.js.org/), men du behöver inte kunna något om dem.

När du är redo att driftsätta till produktion, kör då `npm run build` för att skapa en optimerad build av din app i `build`-mappen. You kan läsa mer om Create React App [i dess README](https://github.com/facebookincubator/create-react-app#create-react-app--) och dess [Användarguide](https://facebook.github.io/create-react-app/).

### Next.js {#nextjs}

[Next.js](https://nextjs.org/) är ett populärt och lättviktsramverk för **statiska och serverrenderade applikationer** byggda med React. Det inkluderar **styling- och routinglösningar** out-of-the-box, och förutsätter att du använder [Node.js](https://nodejs.org/) som servermiljö.

Du kan lära dig Next.js via [dess officiella guide](https://nextjs.org/learn/).

### Gatsby {#gatsby}

[Gatsby](https://www.gatsbyjs.org/) är det bästa sättet att skapa **statiska webbplatser** med React. Det låter dig använda React-komponenter, men matar ut förrenderad HTML och CSS för att garantera snabbast möjliga hämtningstid.

Du kan lära dig Gatsby via [dess officiella guide](https://www.gatsbyjs.org/docs/) och en [samling med startkit](https://www.gatsbyjs.org/docs/gatsby-starters/).

### Mer flexibla verktygskedjor {#more-flexible-toolchains}

De följande verktygskedjorna erbjuder mer flexibilitet och valfrihet. Vi rekommenderar dem till mer erfarna användare:

- **[Neutrino](https://neutrinojs.org/)** kombinerar styrkan hos [webpack](https://webpack.js.org/) med enkelheten av presets, och inkluderar en preset för [React-appar](https://neutrinojs.org/packages/react/) och [React-komponenter](https://neutrinojs.org/packages/react-components/).

- **[Parcel](https://parceljs.org/)** är en snabb nollkonfigurationspaketerare för webbapplikationer, som [fungerar med React](https://parceljs.org/recipes.html#react).

- **[Razzle](https://github.com/jaredpalmer/razzle)** är ett serverrenderingsramverk som inte kräver någon konfiguration, men erbjuder mer flexibilitet än Next.js.

## Skapa en verktygskedja från scratch {#creating-a-toolchain-from-scratch}

En JavaScript-verktygskedja består vanligtvis av:

* En **pakethanterare**, såsom [Yarn](https://yarnpkg.com/) eller [npm](https://www.npmjs.com/), vilket låter dig dra nytta av ett brett ekosystem av tredjepartspaket, och enkelt kunna installera och uppdatera dem.

* En **paketerare**, såsom [webpack](https://webpack.js.org/) eller [Parcel](https://parceljs.org/), vilket låter dig skriva modulär kod och buntar sedan ihop allt till små paket för att optimera hämtningstid.

* En **kompilerare**, såsom [Babel](https://babeljs.io/), vilket låter dig skriva modern JavaScript-kod som sedan även fungerar i äldre webbläsare.

Om du föredrar att sätta upp din egen JavaScript-verktygskedja från scratch, [kolla då den här guiden](https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658) som återskapar några av funktionerna i Create React App.

Glöm inte att försäkra dig om att din egna verktygskedja [är korrekt konfigurerad för produktion](/docs/optimizing-performance.html#use-the-production-build).
