
# Installation de Meteor

tapper la commande suivante dans un shell

```
$ curl https://install.meteor.com/ | sh
```

celà va installer meteor et ses dépendances.

# Creation du projet

Attention mongoDB ne fonctionne pas sur NFS. Il faut donc que le projet soit installer
sur un dossier local ou dans `/tmp`.

```
$ meteor create chat
```


celà créer un dossier `chat` contenant un projet de base.

Ce projet contien 3 fichiers, chat.html, chat.css et chat.js

# Demarer le server

Il suffit d'exécuter la commande suivante dans le dossier `chat`

```
meteor
```

Il modifier les fichiers qui en on besoin (es6, less, etc) puis va instancier un serveur nodejs ainsi qu'une base mongo.

Une fois fini il vous sufit d'ouvrir un navigateur web à l'adress [localhost:3000](http://localhost:3000).

vous trouverai une page avec un bouton et un conteur de clique sur le bouton.

# Exploration du projet par défaut.

Par défaut un projet meteor contiens 3 fichiers.

`chat.css` qui est vide.

`chat.html` ne contien pas le doctype ni la balise `html`
`{{> hello}}` permet d'insérer le template `hello` à cette endroit.
Un templat est défini par une balse `template` et doit contenir l'attribue `name` qui
définira le nom du template.

`chat.js`contien tout le code. aussi bien client que serveur. C'est pour ça qu'il y a un test sur `Meteor.isClient`, cela permet qu'il soit exécuté uniquement sur le client.
`Session` permet de stocker des valeurs réactive. c'est ce qui permetà à meteor de mettre à jour l'html lorsque l'utilisateur clique sur le bouton. Pour plus d'information sur les sessions, allez [voir la documentation](http://docs.meteor.com/#/full/session).

Il contien aussi un helpers qui permet au template d'afficher la varriable `counter` et un event qui permet d'incrémenter `counter`.

Exercice:

1. Modifier le helpers pour qu'il affiche le double de la valeur de  `counter`.
2. Modifier l'events pour qu'il incrémente de 3 `counter`.


lorsque l'on modifie les fichier le serveur est automatiquement relancer à fin d'afficher

# Récuperation du code de l'ancien chat

Ajouter le css suivant dans le fichier `chat.css`

```
* { margin: 0; padding: 0; box-sizing: border-box; }
body { font: 13px Helvetica, Arial; }
form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }
form input { border: 0; padding: 10px; width: 90%; margin-right: .5%; }
form button { width: 9%; background: rgb(130, 224, 255); border: none; padding: 10px; }
#messages { list-style-type: none; margin: 0; padding: 0; }
#messages li { padding: 5px 10px; }
#messages li:nth-child(odd) { background: #eee; }
```

Dans le fichier `chat.html` supprimer le titre puis remplacez le code du template `hello` par le suivant:

```
<ul id="messages">
  {{#each messages}}
    {{this}}
  {{/each}}
</ul>
<form action="">
  <input id="m" autocomplete="off" /><button>Send</button>
</form>
```

Pour plus d'information regarder la documentation des [template](http://docs.meteor.com/#/full/templates_api) et de [blaze](http://docs.meteor.com/#/full/blaze_render).

# Utilisation de MongoDB (Collection)

Ajouter en début du fichier JS
```
const Messages = new Mongo.Collection('messages')
```

ajouter dans le helpers:
```
messages() {
  return Messages.find();
}
```

Dans votre browser ouvrer la console de debug (F12)

vous avez un accès à la collection et vous pouvez ajouter modifier des données dans la base.
Voir la [doc de Meteor](http://docs.meteor.com/#/full/mongo_collection)

Exercice:

1. Ajouter/modifier/supprimer des messages.
2. ajouter l'event `change` sur le form pour qu'il ajoute un message.

# rejoindre le chat d'un de vos camarade.

Remarquer qu'on a un accès total au donnée.

Exercice:

Ajouter/modifier/supprimer des messages.

# retirer insecure

Pour lister les packages du projet.

```
meteor list
```

Il faut retirer le package insecure.

```
meteor remove insecure
```


refaire fonctionner le chat. Pour cela changer l'event et ne plus faire directement un `messages.insert` mais utiliser `Meteor.methods` et `Meteor.call`, [voir methods](http://docs.meteor.com/#/full/meteor_methods).

# retirer autopublish

Un autre package ne doit pas être concervé lorsque le projet deviens plus sérieux. C'est le package `autopublish`.

```
meteor remove autopublish
```

Pour refaire fonctionner le chat il faut que le serveur envoie les messages et que le client souscrit au messages. Pour ça lisez regarder dans la [documentation](http://docs.meteor.com/#/full/meteor_publish) comment ce mechanisme fonctionne.


# Meteor.user() avec accounts-password

Meteor suppor un système d'utilisateur. Il suffit d'ajouter un package d'autentification. (accounts-password, accounts-facebook, etc)

Pour trouver les autres packages `meteor search accounts`.

Vous pouvez utiliser les packages `accounts-password` et `accounts-ui`, ce qui vous permettera d'utilisé le hepers `{{loginButtons}}` a fin de générer facilement l'autentification des utilisateurs.

Pour plus de détails pour le système de login regarder dans la section [accounts](http://docs.meteor.com/#/full/accounts_api) de la documentation.


# autres tutoriel :

Tuto fait lors de la sortie de la version 1.0.3

https://docs.google.com/document/d/1QRrqmuTZMnJISFEJ2YsT9JkMKZ292KbNP9tG17cBie0/edit#heading=h.i72wxdvkq2pp
