# Création d'un composant React

## Génération d'un projet React pour les citations des Simpsons

Dans ton terminal, assure-toi de te placer au bon endroit dans ton espace de travail habituel, là où tu veux créer ton application (Ex : ~/Quests/React/)

React vient avec un générateur de projet ```npx create-react-app```. Utilise-le pour générer ton application React.

Une fois l'application affichée dans ton navigateur, tu peux tester la modification de la page d'accueil qui se recharge automatiquement quand on sauvegarde le fichier. Accède au fichier ```src/App.js``` et modifie le titre et le texte affichés par l'application.

## Créer un composant

Dans un premier temps, nous allons travailler à l'affichage d'une citation de la célèbre série des Simpsons, composée d'une image et d'un texte. Pour cela, tu vas créer un composant Quote qui permet d'ajouter à la page les éléments suivants (il s'agit du code HTML que nous voulons ajouter à notre application) :

```
<figure>
    <img
      src="https://cdn.glitch.com/3c3ffadc-3406-4440-bb95-d40ec8fcde72%2FNelsonMuntz.png?1497567511185"
      alt="Nelson Muntz"
    />
    <figcaption>
      <blockquote>
        Shoplifting is a victimless crime, like punching someone in the dark.
      </blockquote>
      <cite>Nelson Muntz</cite>
    </figcaption>
  </figure>
```

Pour accomplir cela, crée le fichier ```Quote.js``` dans le dossier src de ton projet, avec le contenu suivant:

```
// src/Quote.js
import React, { Component } from "react";

export default class Quote extends Component {
  render() {
    return (
      <figure>
        <img
          src="https://cdn.glitch.com/3c3ffadc-3406-4440-bb95-d40ec8fcde72%2FNelsonMuntz.png?1497567511185"
          alt="Nelson Muntz"
        />
        <figcaption>
          <blockquote>
            Shoplifting is a victimless crime, like punching someone in the dark.
          </blockquote>
          <cite>Nelson Muntz</cite>
        </figcaption>
      </figure>
    );
  }
}
```

Ici, le composant Quote est défini par une fonction qui retourne du code JSX.

Pour afficher ce composant à l'écran, il faut l'appeler dans le code JSX retourné par la méthode ```render``` du composant ```App.js```.

Modifie le fichier ```src/App.js``` :

```
// src/App.js
import React, { Component } from "react";
import logo from "./logo.svg";
import "./App.css";

import Quote from "./Quote";

export default class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Simpsons Quotes</h1>
        </header>
        <Quote />
      </div>
    );
  }
}
```

## Rendre l'affichage des valeurs dynamique grâce aux props

Nous allons rendre le composant Quote capable d'afficher différentes citations, en lui passant des valeurs en attributs depuis le composant App. Les attributs sont quote, character et image.

Modifie le fichier ```App.js```:

```
import React, { Component } from "react";
import logo from "./logo.svg";
import "./App.css";

import Quote from "./Quote";

export default class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Simpsons Quotes</h1>
        </header>
        <Quote
          quote="I believe the children are the future... Unless we stop them now!"
          character="Homer Simpson"
          image="https://cdn.glitch.com/3c3ffadc-3406-4440-bb95-d40ec8fcde72%2FHomerSimpson.png?1497567511939"
        />
        <Quote
          quote="Me fail English? That's unpossible"
          character="Ralph Wiggum"
          image="https://cdn.glitch.com/3c3ffadc-3406-4440-bb95-d40ec8fcde72%2FRalphWiggum.png?1497567511523"
        />
      </div>
    );
  }
}
```

Pour l'instant, cela affichera dans le navigateur deux fois la même citation, car nous n'utilisons pas les valeurs de nos attributs dans le composant Quote.

Cependant, tu peux vérifier que ces valeurs sont bien passées aux composants Quote en inspectant ton application avec ReactDev tools.

Inspecte ton application. Tu dois pouvoir trouver un objet Props qui contient ces valeurs en sélectionnant un composant Quote.

Pour afficher ces valeurs, nous allons modifier le code de ```Quote.js``` :

```
import React, { Component } from "react";

export default class Quote extends Component {
  render() {
    return (
      <figure>
        <img src={props.image} alt={props.character} />
        <figcaption>
          <blockquote>{props.quote}</blockquote>
          <cite>{props.character}</cite>
        </figcaption>
      </figure>
    );
  }
}
```

Teste cette écriture alternative qui est très souvent utilisée. On y déstructure l'objet ```props``` directement en plusieurs variables ```quote```, ```character``` et ```image```.

```
import React, { Component } from "react";

export default class Quote extends Component {
  render() {
    return (
      <figure>
        <img src={image} alt={character} />
        <figcaption>
          <blockquote>{quote}</blockquote>
          <cite>{character}</cite>
        </figcaption>
      </figure>
    );
  }
}
```

 Vérifie dans ton navigateur que les valeurs sont bien affichées.

## Challenge
Créer et afficher un composant Voyage

* Créer une nouvelle application React nommée my-travels grâce à create-react-app

* Créer un composant Travel pour afficher les propriétés suivantes:
  * destination
  * country
  * photo
  * distance

* Affiche deux fois ce composant avec des valeurs différentes.

