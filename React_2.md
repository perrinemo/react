# Afficher une liste de composants

## Créer un composant pour afficher la liste

Dans l'application simpsons-quotes, crée le fichier ```Quotes.js``` et appelle ce composant dans ```App.js```

```
// src/Quotes.js
import React, { Component } from "react";

const quotes = [
  {
    quote:
      "Facts are meaningless. You could use facts to prove anything that's even remotely true.",
    character: "Homer Simpson",
    image:
      "https://cdn.glitch.com/3c3ffadc-3406-4440-bb95-d40ec8fcde72%2FHomerSimpson.png?1497567511939"
  },
  {
    quote: "Nothing you say can upset us. We're the MTV generation.",
    character: "Bart Simpson",
    image:
      "https://cdn.glitch.com/3c3ffadc-3406-4440-bb95-d40ec8fcde72%2FBartSimpson.png?1497567511638"
  },
  {
    quote: "That's where I saw the leprechaun...He told me to burn things.",
    character: "Ralph Wiggum",
    image:
      "https://cdn.glitch.com/3c3ffadc-3406-4440-bb95-d40ec8fcde72%2FRalphWiggum.png?1497567511523"
  },
  {
    quote:
      "Hello, Simpson. I'm riding the bus today because Mother hid my car keys to punish me for talking to a woman on the phone. She was right to do it.",
    character: "Principal Skinner",
    image:
      "https://cdn.glitch.com/3c3ffadc-3406-4440-bb95-d40ec8fcde72%2FSeymourSkinner.png?1497567511460"
  }
];

export default class Quote extends Component {
  render() {
    return (
      <div>
        {quotes.map(quote => (
          <img src={quote.image} />
        ))}
      </div>
    )
  }
}
```

La variable ```quotes``` contient un tableau JavasScript d'objets contenant les données d'une citation
(```quote```, ```character``` et ```image```).

La fonction suivante:

```
quote => <img src={quote.image} />
```

est passée en argument à la méthode map.

Le résultat de map est un tableau d'éléments <img> qui auront dans leur attribut src la valeur de la propriété image de chaque objet du tableau quotes.

Vérifie que l'application affiche bien les quatre personnages.

Profites-en pour jeter un coup d'oeil à la console (tu la gardes toujours ouverte, n'est-ce pas ?). Tu dois voir un avertissement (en rouge) dans la console :

```
index.js:2178 Warning: Each child in an array or iterator should have a unique "key" prop.

Check the render method of `Quotes`. See https://fb.me/react-warning-keys for more information.
```
C'est un avertissement qu'on rencontre souvent, mais qui n'empêche pas la liste de s'afficher. Suis le lien qui t'est donné. Sous la section "Keys", l'un des exemples te donne la solution à ce problème.

## Afficher une liste de composants

Tu vas réutiliser le composant Quote créé dans la quête précédente.

Dans le composant Quotes remplace la fonction

```quote => <img src={quote.image} />```

par

```
quote => (
  <Quote quote={quote.quote} image={quote.image} character={quote.character} />
)
```

L'application doit maintenant afficher la liste des citations correspondant au tableau quotes.

En utilisant le spread operator, tu peux décomposer l'objet pour arriver au même résultat.

```quote => <Quote {...quote} />```

## Challenge
* Créer et afficher un composant Travels

* Dans l'application my-travels créée lors de la quête précédente, ajoute un composant Travels qui contiendra un tableau (les données) de 5 voyages et affichera la liste en réutilisant le composant Travel de la quête précédente.
