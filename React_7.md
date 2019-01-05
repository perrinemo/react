# POST ton formulaire

## Création du formulaire
Dans un 1er temps, nous allons nous concentrer sur la réalisation du formulaire de notre application.

* FormEmployee.js

On commence par initialiser notre composant avec un state.
```
 constructor(props) {
   super(props);
   this.state = {
     lastname: '',
     firstname: '',
     email: '',
   }
 }
```
Ensuite, nous allons créer notre méthode onChange permettant de mettre à jour nos champs contrôlés de notre formulaire.
```
onChange = (e) => {
 this.setState({
   [e.target.name]: e.target.value,
 });
}
```
Afin de gérer manuellement l'envoi du formulaire, on va devoir "désactiver" le comportement du navigateur lors de la soumission d'un formulaire. On va donc créer notre méthode onSubmit qui sera appelé sur notre formulaire. (voir doc "preventDefault" section "Ressources")
```
submitForm = (e) => {
 e.preventDefault();
}
```
Dans le render, on va créer notre formulaire de la manière suivante :
```
<div className="FormEmployee">
 <h1>Saisi d'un employé</h1>

 <form onSubmit={this.submitForm}>
   <fieldset>
     <legend>Informations</legend>
     <div className="form-data">
       <label htmlFor="lastname">Nom</label>
       <input
         type="text"
         id="lastname"
         name="lastname"
         onChange={this.onChange}
         value={this.state.lastname}
       />
     </div>

     <div className="form-data">
       <label htmlFor="firstname">Prénom</label>
       <input
         type="text"
         id="firstname"
         name="firstname"
         onChange={this.onChange}
         value={this.state.firstname}
       />
     </div>

     <div className="form-data">
       <label htmlFor="email">E-mail</label>
       <input
         type="email"
         id="email"
         name="email"
         onChange={this.onChange}
         value={this.state.email}
       />
     </div>
     <hr />
     <div className="form-data">
       <input type="submit" value="Envoyer" />
     </div>
   </fieldset>
 </form>
</div>
```
Afin de rajouter un peu de "style" à notre formulaire, on peut ajouter le CSS suivant (dans App.css, c'est plus pratique)
```
.FormEmployee {
 width: 400px;
 margin: 0 auto;
 text-align: left;
}

.FormEmployee h1 {
 text-align: center;
}

.FormEmployee .form-data {
 display: flex;
 justify-content: center;
 margin: 5px 0;
}

.FormEmployee .form-data label {
 display: inline-block;
 width: 75px;
 margin-right: 10px;
 text-align: right;
}

.FormEmployee .form-data input:not([type="submit"]) {
 flex-grow: 1;
}
```
  * Importe ton composant dans App.js.
  * Vérifie maintenant que ton formulaire est bien affiché dans ton application.

## Fetch - POST
**Comment j'envoie mes données une fois mon formulaire rempli ?**

Grâce à Fetch, on va pouvoir envoyer notre nouvel employé afin de le sauvegarder dans la base de données.

Si l'on regarde d'un peu plus près fetch, on remarque que l'on peut passer 2 arguments à la méthode :
```
fetch(url, config)
```
On peut donc configurer l'appel à une URL. Voyons ça de plus près !

Je vous conseille de vous rendre sur ce lien c'est un exemple très simple d'envoi de JSON avec fetch.

**Quels paramètres allons nous utiliser?**

* method : permet de surcharger la méthode de la requête HTTP (GET/POST/PUT/DELETE). Par défaut la méthode est "GET"

* headers : permet de définir (entre autres) le type de données que l'on envoie.

* body : définition des données à envoyer

Dans notre exemple, nous aurons donc notre config :
```
const config = {
 method: "POST",
 headers: {
   "Content-Type": "application/json",
 },
 body: JSON.stringify(this.state),
};
```
L'url de notre POST sera :
```
const url = "http://92.175.11.66:3001/api/quests/employees/";
```
Il nous reste plus qu'à faire notre fetch. Dans notre exemple nous afficherons un message dans une alert afin d'indiquer si l'ajout s'est correctement fait ou non.
```
fetch(url, config)
.then(res => res.json())
 .then(res => {
   if (res.error) {
     alert(res.error);
   } else {
     alert(`Employé ajouté avec l'ID ${res}!`);
   }
 }).catch(e => {
   console.error(e);
   alert('Erreur lors de l\'ajout d\'un employé');
 });
 ```
> L'API actuelle est paramétrée pour renvoyer un message d'erreur si l'ajout ne s'est pas fait correctement. Ce n'est pas le cas de toutes les API !!

## Challenge
Insertion de ton film préféré
* Tu vas réaliser une application permettant de saisir ton film préféré afin de partager cette information avec tous tes camarades

  * L'API est disponible sur l'URL suivante : http://92.175.11.66:3001/api/quests/movies/

  * L'application devra comprendre :
 
    * Un formulaire comprenant 3 champs obligatoires
    * Un champ texte pour saisir le nom du film
    * Un champ texte pour saisir une URL correspondant au poster du film
    * Un champ textarea permettant de saisir un commentaire (pourquoi tu aimes ce film? qu'est ce qui t'a marqué? etc...)
    * Un bouton permettant l'envoi du formulaire.
    * Un message s'affiche lorsque le film a bien été enregistré.
