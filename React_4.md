# Le composant contrôlé

##Sans contrôle
Si on affiche un champ ```<input>``` sans spécifier son attribut ```value```, on dit que ce champ est non contrôlé.
```
import React, { Component } from "react";

export default class NameForm extends Component {
  render() {
    return (
      <form>
        <label htmlFor="name">Name :</label>
        <input id="name" type="text" />
      </form>
    );
  }
}
```
La valeur affichée par le champ ```name``` sera celle saisie par l'utilisateur.

Cela pose deux problèmes particuliers:

la valeur n'est pas stockée au niveau du state ;
on ne peut pas réagir à la saisie. Pour lire la valeur saisie par l'utilisateur, il faudra accéder au champ lors de 
la soumission du formulaire, par exemple.
On notera l'utilisation de ```htmlFor``` à la place de ```for``` dans ```label``` pour la même raison qu'on utilise 
```className``` à la place de ```class```.

Copie le code du formulaire dans une application React et appelle le composant dans ```App.js```.

## Prends le contrôle
Un ```input``` est dit contrôlé quand la valeur de son attribut ```value``` est fournie par le composant.

Nous allons modifier ```NameForm``` pour afficher la valeur provenant du ```state```

Ajoute ce constructeur au composant:
```
constructor(props) {
    super(props);
    this.state = { name: "Homer Simpson" };
}
```
Maintenant, modifie l'attribut ```value``` de l'input pour afficher la valeur contenue dans le ```state```

Après cela, le champ de formulaire doit afficher ```Homer Simpson``` et surtout le champ ne doit plus réagir à la saisie 
de l'utilisateur.

## Modifier la valeur dans le state
Un champ non modifiable par l'utilisateur n'est pas très utile. Maintenant que la valeur provient du state, 
il va falloir modifier cette valeur quand l'événement ```onChange``` est propagé.

Pour récupérer cet événement, il va falloir écrire une méthode qui sera notre event handler 
(gestionnaire d'évènements).

Cette méthode prendra l'event en paramètre et permettra d'accéder à ```event.target.value``` qui correspond à 
la saisie de l'utilisateur.

Utilise la documentation officielle en ressource pour modifier le composant ```NameForm```, 
et faire en sorte que la saisie soit maintenant possible dans le formulaire.

Le champ n'est plus modifiable, car c'est le composant qui contrôle sa valeur.

[React Forms](https://reactjs.org/docs/forms.html)

## Challenge
Affichage synchronisé avec la saisie
* Tu vas réaliser un formulaire qui va afficher dans un élément <h1> le contenu saisi dans l'input.
* Pour soumettre ce challenge, tu vas utiliser codesanbox.io, fork de sandbox : [https://codesandbox.io/s/507j3q40pn](https://codesandbox.io/s/507j3q40pn)
* Modifie le bon fichier pour que la saisie s'affiche en même temps dans l'input et dans le H1.
* Transforme la saisie de l'utilisateur en traitant la valeur, avant de la stocker dans le state pour interdire la saisie du caractère "*".
