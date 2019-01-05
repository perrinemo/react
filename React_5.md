# Fetch - Ou comment consommer une API avec React ?

## Création des composants
* GenerateEmployee.js (bouton permettant de choisir aléatoirement un employé)
```
import  React  from  'react';

export  default class GenerateEmployee {
  render() {
    return (
        <div  className="GenerateEmployee">
          <button  onClick={selectEmployee}>Get employee</button>
        </div>
    );
  }
}
```
* DisplayEmployee.js
```
import  React  from  'react';

export  default class DisplayEmployee {
  render() {
    return (
        <div  className="DisplayEmployee">
            <img  src={employee.picture.medium}  alt="picture"  />
            <ul>
                <li>Gender : {employee.gender}</li>
                <li>
                    Name : {employee.name.title}  {employee.name.last}{' '}
                    {employee.name.first}
                </li>
                <li>E-mail : {employee.email}</li>
                <li>
                    Location : {employee.location.street},
                    {employee.location.postcode}{' '}{employee.location.city}
                </li>
            </ul>
        </div>
    );
  }
}
```
Nous allons avoir besoin maintenant d'un employé "test" pour voir si nos composants 
s'affichent bien ! Tu va donc afficher l'employé suivant :
```
const  sampleEmployee = {
    gender:  'male',
    name: {
        title:  'mr',
        first:  'mathys',
        last:  'aubert'
    },
    location: {
        street:  '9467 rue gasparin',
        city:  'perpignan',
        postcode:  '90208'
    },
    email:  'mathys.aubert@example.com',
    picture: {
        medium:  'https://randomuser.me/api/portraits/med/men/66.jpg'
    }
};

```
Depuis App.js on va appeler nos différents composants et vérifier si l'affichage s'effectue correctement :

* App.js
* On importe nos composants
```
import  GenerateEmployee  from  './GenerateEmployee';
import  DisplayEmployee  from  './DisplayEmployee';
```
* On créé notre employé de test
```
const  sampleEmployee = {...}
```
* Dans le render on affiche nos composants
```
<GenerateEmployee  />
<DisplayEmployee  employee={sampleEmployee}  />
```
* Vérifie maintenant que ton employé est bien affiché dans ton application

## Introduction à Fetch
Qu'est-ce-que Fetch ?

Comme le dit si bien Mozilla :

> L'API Fetch fournit une interface JavaScript pour l'accès et la manipulation des parties de la 
pipeline HTTP, comme les requêtes et les réponses. Cela fournit aussi une méthode globale fetch() 
qui procure un moyen facile et logique de récupérer des ressources à travers le réseau de manière asynchrone.

Pour faire simple, en JS nous avons accès à une méthode fetch permettant d'envoyer ou de recevoir des informations 
depuis une URL passée en paramètre.

**Exemple**

Imaginons qu'une API sur l'url http://my-api.com/1 nous renvoie un JSON formaté de la manière suivante :
```
{ "name": "John Smith"}
```
Grâce à fetch, on va pouvoir récupérer cette information :
```
fetch('http://my-api.com/1')
    .then(results  =>  results.json()) // conversion du résultat en JSON
    .then(data  => {
        console.log(data.name); // affiche "John Smith"
});
```
**Pourquoi doit-on convertir le 1er résultat??**

Lorsqu'on fait appel à une requête via fetch, on reçoit le résultat de la requête dans un format particulier, 
c'est un objet Response.

Cet objet n'est pas utilisable en tant que données brutes. Nous appelons donc une méthode particulière, .json(). 
Si le "corps" de la réponse contient du JSON, cette méthode va le "convertir" en un objet ou tableau JavaScript.

> !! Ceci est uniquement valable si les données reçues sont convertibles en JSON, et qu'aucune erreur n'a été remontée 
pendant l'appel à l'URL. !!

## Récupération des données
Pour cela, nous allons inclure dans nos composants un système de requêtes HTTP.

Afin de faciliter le développement de notre application nous allons utiliser une API externe permettant de 
générer aléatoirement un employé.

[Random User](https://randomuser.me)

Si tu veux tester l'API, lance ton navigateur et colle l'url suivante. Tu devrais avoir d'afficher un JSON 
représentant une personne.

> [https://randomuser.me/api?nat=fr](https://randomuser.me/api?nat=fr)

Bien, maintenant que les présentations entre l'API et toi sont faites, on va transformer nos composants pour 
qu'ils récupèrent les données depuis cette API !

La 1ère question qui se pose est : **Quel composant doit faire l'appel à cette ressource ?**

Et bien généralement ça sera notre composant "parent" qui va s'occuper de faire les appels et de re-distribuer 
les informations aux composants "enfants". Donc dans notre cas, ça sera App.js qui fera cette récupération.

* On va d'abord initialiser un state à notre composant qui recevra l'employé
```
constructor(props) {
  super(props);
  this.state = {
    // on peut mettre notre sampleEmployee par défaut
    // afin d'avoir toujours un employé d'affiché
    employee:  sampleEmployee
  };
}
```
* Ensuite, on va récupérer l'action de notre clic sur notre bouton
```
<GenerateEmployee  selectEmployee={() =>  this.getEmployee()}  />
```
Il faut maintenant déclarer la méthode getEmployee dans notre composant App qui fera l'appel à notre API
```
getEmployee() {
    // Récupération de l'employé via fetch
    fetch("https://randomuser.me/api?nat=fr")
      .then(response  =>  response.json())
      .then(data  => {
        // Une fois les données récupérées, on va mettre à jour notre state avec les nouvelles données
        this.setState({
          employee:  data.results[0],
        });
    });
}
```
* Il nous reste juste à modifier l'appel au composant DisplayEmployee afin qu'il affiche l'employé récupéré
```
<DisplayEmployee  employee={this.state.employee}  />
```
Et voilà ! À chaque fois que vous cliquez sur le bouton "Get employee" un nouvel employé est affiché sur votre application!

Il se peut que l'affichage ne se fasse pas immédiatement, cela est dû au temps de latence entre l'appel à l'API 
et la récupération des données.

## Challenge

Affichage d'une citation des Simpsons
* Tu vas réaliser une application permettant de récupérer aléatoirement une citation tiré des Simpsons
* Utilise l'[API The Simpsons Quote](https://thesimpsonsquoteapi.glitch.me/)
* L'application devra comprendre :
  * Un bouton permettant de récupérer une citation
  * Une zone où est affiché le nom du personnage, la photo ainsi que la citation
