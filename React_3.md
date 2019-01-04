# Le state

## Appliquer une classe de façon conditionnelle
Avant d'aborder le state proprement dit, on va voir comment un composant peut faire varier son rendu, en fonction de propriétés 
qui lui sont passées. On va pour cela partir d'un exemple simple.

Dans le projet ```simpsons-quotes``` initié précédemment, tu vas créer un composant Lamp, pour afficher une lampe, composée 
d'une ampoule et d'un interrupteur. D'abord un peu de copier-coller, et ensuite les explicatons !

Crée le fichier ```Lamp.js``` dans le dossier src, avec le contenu suivant :
```
// Lamp.js
import React, { Component } from 'react';

export default class Lamp extends Component {
  render() {
    const light = this.props.on ? 'on' : 'off';
    return (
      <div className="Lamp">
        <button className={light}>{light.toUpperCase()}</button>
        <figure className={light} />
      </div>
    );
  }
}
```

Dans ce composant, le ```<button>``` représente l'interrupteur, et la ```<figure>```, l'ampoule.

Ajoute ensuite cette ligne, dans la partie ```<head>``` du fichier ```index.html```, qui se trouve dans le répertoire public :
```
<link rel="stylesheet" href="https://drive.google.com/uc?export=view&id=1WyjM4ZBgcWAR2rMmrulmY_wfeRVKmu9R">
```

Comme l'attribut rel="stylesheet" le suggère, il s'agit d'une feuille de style. On la charge depuis une URL externe, car elle 
est trop volumineuse pour être insérée ici. Elle contient des déclarations CSS permettant de modifier le rendu de l'interrupteur 
et de l'ampoule, suivant que la lampe est allumée ou éteinte, via des classes on et off.

Enfin, on appelle le composant Lamp depuis App, à deux reprises, en ajoutant ceci dans le render de App, par exemple juste 
au-dessus de ```<Quotes />``` :
```
  <Lamp on />
  <Lamp />
```
Tu devrais voir apparaître deux lampes, la première allumée, la seconde éteinte. Note qu'il est préférable d'écrire
```<Lamp on />``` plutôt que ```<Lamp on={true} />``` dans le cas d'une propriété booléenne : elle vaudra implicitement true. 
Le fait de ne pas la mettre sur le 2ème composant Lamp fait qu'il ne recevra aucune valeur, donc la lampe restera éteinte.

On n'utilise pas encore le state : le composant parent App passe à chaque Lamp une propriété on, qui contient un booléen indiquant 
si la lampe est allumée ou éteinte (undefined étant évalué comme false).

Dans le render du composant Lamp, on commence par récupérer cette propriété. Puis, en fonction de la valeur true ou false, on 
détermine une classe CSS on ou off.

Enfin, quand on retourne le contenu à afficher, cette classe est attribuée au bouton et au label via className. Pourquoi 
className plutôt que class en HTML ? C'est dû à la nature de JSX, qui permet de mélanger des balises et du JavaScript, mais 
qui est transformé ensuite en pur JS. Les attributs des composants React ne doivent donc pas comporter certains mots-clés qui 
sont utilisés par le langage. Or class est déjà utilisé pour déclarer une classe au sens JavaScript, tel que notre composant Lamp. 
On utilise donc className à la place. Il en va de même pour d'autres attributs, tel que le for qu'on ajoute sur un label 
(pour les formulaires), et qui devient htmlFor en JSX, pour ne pas faire de confusion avec le for des boucles JavaScript.

## Réagir à une interaction utilisateur
Avant l'apparition de JavaScript, les navigateurs se contentaient d'afficher "bêtement" une page HTML, sans trop de possibilités 
d'interaction, à part la soumission de formulaires et le clic sur des liens.

Avec JavaScript, est apparue la possibilité de réagir aux actions de l'utilisateur. Toutes les interactions entre la page et 
l'utilisateur sont basées sur un système d'évènements : à chaque fois que l'utilisateur effectue une action, un évènement est généré.

Comment réagir à un certain évènement en React ? En donnant à une balise un attribut, qui correspond au type d'évènement que 
l'on veut détecter.

Tu vas voir cela en pratique : dans le fichier ```Lamp.js```, ajoute l'attribut ```onClick={() => alert('Button clicked')}``` à la balise 
```<button>```, qui doit maintenant ressembler à ceci (n'hésite pas à passer à la ligne pour chaque attribut) :
```
<button
  onClick={() => alert('Button clicked')}
  className={light}
>
  {light.toUpperCase()}
</button>
```
La propriété ```onClick``` ajoutée au ```<button>``` a pour valeur... une fonction ! On peut en effet passer toutes sortes de 
valeurs dans les propriétés des composants.

Cette fonction va être appelée à chaque fois qu'on clique sur le bouton, ce qui est reflété par le nom ```onClick``` : "sur un clic" 
ou "lors d'un clic", on appelle la fonction passée comme valeur.

Au passage, il existe d'autres attributs pour réagir à des évènements de différents types : ```onInput``` pour détecter une saisie 
dans un champ de formulaire, ```onSubmit``` pour détecter une soumission de formulaire, etc. Tu en découvriras plusieurs au fil des 
prochaines quêtes, mais sache que la liste est longue et que nous ne les utiliserons pas tous pour le moment.

## Ajouter un state au composant
Tu as maintenant vu plusieurs exemples de réutilisation d'un même composant à plusieurs reprises : le composant donne une certaine 
structure, identique entre les différentes "instances" du composant, mais le rendu de chaque instance peut varier, en fonction des 
propriétés qu'on lui passe quand on l'appelle.

Ces propriétés sont "imposées" au composant enfant par son composant parent, et il ne peut pas les modifier. On les appelle props.

Le state - on garde le terme anglais pour une bonne raison - permet à un composant de manipuler des données qui lui sont propres, 
et qu'il est susceptible de modifier.

Ajoute un state initial au composant Lamp, en ajoutant ces 3 lignes juste au-dessus de render - mais toujours à l'intérieur des 
accolades qui délimitent la classe :
```
constructor(props) {
  super(props);
  this.state = {
    on: false
  };
}
```
Le state est un objet, qui peut contenir plusieurs valeurs : ici, on a juste un booléen, affecté à la clé on. On va maintenant 
utiliser le on du state, au lieu de la propriété on passée par le composant parent.

Dans la méthode render, remplace :
```
const light = this.props.on ? 'on' : 'off';
```
par
```
const light = this.state.on ? 'on' : 'off';
```
Le rendu de la lampe dépend désormais du state, et plus des props !

Tu peux le vérifier en te rendant sous l'onglet React des outils de développement, pour trouver les composants Lamp. 
Choisis l'un deux, et manipule la propriété on du state via la case à cocher correspondante : l'affichage de la lampe 
correspondante change. Et tu peux effectivement constater que changer la propriété on, passée depuis le parent, ne change plus rien.

## Modifier le state via setState
On va finir d'assembler ce qu'on a fait auparavant : on va faire en sorte que le clic sur l'interrupteur modifie le state. 
Au lieu d'attribuer une fonction anonyme comme valeur à l'attribut ```onClick```, tu vas lui attribuer une méthode de la classe Lamp. 
Ajoute ces lignes, entre les 3 lignes d'initialisation du state, et render :
```
handleClick = () => {
  console.log('Button clicked');
};
```
Puis remplace l'attribut ```onClick``` du bouton, en y mettant ```onClick={this.handleClick}```. Si tu sauves à ce stade, le changement 
n'est pas spectaculaire : on affiche un message dans la console sur un clic.

Dans ```handleClick```, on va maintenant remplacer ce ```console.log``` par le code qui permet de modifier le state. On pourrait être 
tenté d'écrire ceci :
```
// Négation de la valeur actuelle, et affectation comme nouvelle valeur
this.state = { on: !this.state.on };
```
Oui... mais non ! Il ne faut surtout pas écrire cela. En effet, le seul endroit où l'on peut écrire ```this.state = {...}``` est 
le constructeur, appelé lorsque le composant est inséré dans la page. Après, c'est interdit !

Il faut, à la place, utiliser la méthode ```setState``` pour modifier le state. Contrairement à ```handleClick``` que tu as écrite 
toi-même, et semblablement à render, la méthode ```setState``` existe déjà sur ton composant, du simple fait que la classe Lamp 
hérite de ```React.Component```. Elle s'appelle en lui passant en paramètre un objet, contenant les clés à modifier dans le state, 
et les valeurs associées. Ecris cette ligne dans ```handleClick``` :
```
this.setState({ on: !this.state.on });
```
Eurêka ! Les interrupteurs de nos lampes fonctionnent. Les deux composants Lamp étant des entités distinctes, chacun a son 
propre state, indépendant de celui de l'autre.

À ce stade, on pourrait supprimer la propriété on passée de App à chacun des deux Lamp. Mais on va en profite pour voir une 
dernière chose : on n'est pas obligé d'écrire la valeur initiale du state "en dur" (avec on à false dans le cas présent).

On peut très bien utiliser la propriété on comme valeur initiale du on du state. Initialise le state dans le constructeur 
comme ceci :
```
this.state = {
  on: props.on
};
```
Les lampes ayant reçu des propriétés différentes, la valeur initiale de on dans leur state est différente.

## Challenge
Homer prend une pause...
Cette fois-ci, on reste dans l'application simpsons-quotes pour le challenge.

Inspire-toi de ce qu'on vient de faire, pour :

* Ajouter un state au composant App, contenant une seule propriété working, booléenne, indiquant si Homer travaille (true) ou s'il est en pause (false).
* Ajouter un bouton permettant de modifier la valeur de working dans le state.
En fonction de cette valeur, modifier l'animation du symbole "Atome", qui est le logo de React. Tu peux pour cela ajouter une classe juste sous App-logo, dans App.css, et changer l'animation du logo en fonction du state : soit l'accélérer, soit créer un autre @keyframes en remplaçant transform: rotate(...) par transform: scale(...). Tu peux aussi tenter autre chose d'original, du moment que le state de App est modifié par le bouton, et que l'affichage change en conséquence.
