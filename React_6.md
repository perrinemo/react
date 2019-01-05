# Filter l'affichage d'une liste

## Filtrer selon un critère
```
student  =>  student.note >= 10
```
Pour appliquer le filtre à chaque student du tableau il faut appeler la méthode filter avec en argument la fonction ci-dessus. Comme il faut filtrer le tableau avant de l'afficher, il faut appeler cette méthode avant d'appeler la méthode map

Modifie l'affichage du tableau dans la partie Graduates:
```
this.state.students.filter(student  =>  student.note >= 10).map ...
```
La liste Graduates ne doit plus afficher que les students concernés.

Le [sandbox](https://codesandbox.io/s/66v1409nz) à forker pour faire ton test

## Appliquer un critère selon un choix utilisateur
Cette fois ci le composant App comporte un bouton qui change une propriété du state.
Identifie d'abord quelle valeur du state est modifiée quand on clique sur le bouton.

Le critère doit maintenant prendre en compte cette valeur du state.

Si state.showFailedOnly est false on affichera tous les students.

Si state.showFailedOnly est true on affichera seulement les students avec une note inférieure à 10.

Si on applique cela à un seul student cela donne :
J'affiche le student si showFailedOnly est faux OU si sa note est < 10
La fonction appliquée par filter peut donc s'écrire:
```
student  => !this.state.showFailedOnly || student.note < 10
```
Modifie le code qui affiche la liste pour appliquer ce filtre, le contenu de la liste doit changer quand tu cliques sur le bouton.

Le [sandbox](https://codesandbox.io/s/rlz9k52y4n) à forker pour faire ton test

## Challenge
Créer un filtre Simpsons activable
* Fork le sandbox suivant [filter-simpsons](https://codesandbox.io/s/ykpkq94qrx)

* Examine le code de App.js, le tableau de citations qui est dans le state et comment les citations sont affichées.

* Ajoute un bouton "Simpsons Only"

* Au clic sur ce bouton la liste de citations doit être filtrée et afficher seulement les citations des Simpsons.
