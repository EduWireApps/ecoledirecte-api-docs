# EcoleDirecte Api Documentation

## Introduction


Bienvenue sur la documentation non officielle de l'api d'ecoledirecte.

Si jamais cette documentation vient a être défaillante, merci de faire une issue avec les problèmes rencontrés

(Si jamais vous avez des idées de formatage, hésitez pas à me le faire savoir [MaitreRouge#6916](https://discord.gg/EHM7jubSvE))


## Index


Juste un rapide sommaire pour naviguer plus facilement dans la documentation.

- [Format de la documentation](#format-de-la-documentation)
- [Utilisation de l'API](#utilisation-de-lapi)
  - [Requêtes authentifiées](#requêtes-authentifiées)
  - [Codes erreur](#codes-erreur)
  - [Servers.json](#serversjson)

* [Login](#login)
  * [Account objects](#accounts-objects)
    * [Modules](#account-modules)
* [Élève](#élève)
  * [Timeline](#timeline)
  * [Timeline commune](#timeline-commune)
  * [Sondages](#sondages)
  * [Formulaires](#formulaires)
  * [Visios](#visios)
  * [Emploi du temps](#emploi-du-temps)
  * [Cahier de texte](#cahier-de-texte)
  * [Vie scolaire](#vie-scolaire)
  * [Carnet de correspondance](#carnet-de-correspondance)
  * [Documents administratifs](#documents-administratifs)


## Format de la documentation

La base de l'api élève est ``https://api.ecoledirecte.com/v3/``. Toutes les URLs relatives sont relatives à cette base.

Les requêtes prennent généralement des paramètres soit en *query string* dans l'URL soit en JSON dans le corps de la requête. Ces paramètres sont encodés au format `application/x-www-form-urlencoded`, bien que cet encodage soit totalement optionnel pour le corps de la requête.

**Note** : On peut utiliser la fonction `encodeURIComponent()` en JavaScript et `urllib.parse.quote()` en Python pour encoder ces paramètres.

**Exemple** : Votre mot de passe est `JaimeLesK++kies&Ynotes` il faudra envoyer `JaimeLesK%2B%2Bies%26`

**Important** : Toutes les requêtes sont faites en `POST`. Elles prennent aussi un paramètre `verbe` utilisé pour spécifier le verbe HTTP. Ici ce paramètre est omis et la méthode HTTP indiquée est en réalité la valeur du paramètre `verbe` (l'exemple est plus clair).

Les réponses suivent généralement le format suivant, et tout schéma de réponse donné correspond en réalité à celui de la valeur de `data` :

```jsonc
{
  "host": "HTTP<n° serveur>",
  "code": 200,
  "token": "<token>",
  "message": "", // Rarement présent
  "data": {...},
}
```

### Exemple

__GET__ `/v3/eleves/{eleve.id}/messages/{message.id}.awp`

Avec les paramètres de recherche `mode=destinataire` et les paramètres en JSON `{ "anneeMessages": "2021-2022" }`

Correspond à la requête

__POST__ `https://api.ecoledirecte.com/v3/E/{élève.id}/visios.awp?verbe=get&mode=destinataire` avec le corps `data={ "anneeMessages": "2021-2022" }` ou bien sous forme encodée `data=%7B%20%22anneeMessages%22%3A%20%222021-2022%22%20%7D`


## Utilisation de l'API

### NOTE IMPORTANTE :

Avant tout, merci d'ajouter un user-agent dans vos headers pour "faire croire" à ED que vous utilisez un "vrai" navigateur pour faire vos requetes !

Il faut savoir que si vous utilisez un useragent pour obtenir un token, il faudra utiliser le même useragent avec ce token. Si jamais un autre UA est utilisé, le token sera invlidé.

Si vous n'avez pas d'idée en voici un :
```
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36
```

### Requêtes authentifiées

Cela concerne toutes les routes à l'exception de `/login`. Il faut passer un `Header` à la requête avec le token (voir [login](#login)):

```
X-Token: <token>
```


### Codes erreur

Liste de differents codes erreur trouvés au fil du temps avec leur description et des solutions pour fixer le pb

*Note : Les requêtes renveront (sauf grosse erreur côté serveur avec une 5xx) toujours dans leur header un code 200, même en cas d'erreur*

```
Code: 505
Problème: Les identifiants donnés à l'api sont erronés
Solution: Il faut vérifier le nom d'utilisateur et/ou le mot de passe (cf Login)

Code: 520
Problème: Le token est invalide
Solution: Il faut regénérer un token (cf Login)

Code: 525
Problème: Le token est expiré
Solution: Il faut regénérer un token (cf Login)

Code: 40129
Problème: Format JSON invalide
Solution: Vérifier que le body de la requete a bien été envoyé en raw (ou plain text) et qu'il respecte le formatage donné dans la documentation
```




### Servers.json

EcoleDirecte n'utilise pas qu'un seul serveur, il est possible d'obtenir des informations sur les serveurs du site en une seule requete.
La raison pour laquelle cette partie est dans la partie référence c'est car cela n'a pas trop d'utilitée a vraiment parler...

*Notes : Il semblerait que le fichier soit statique :c (les pings sont les mêmes depuis que j'ai commecé la doc)*

Voici le lien du fichier : [https://www.ecoledirecte.com/EDCluster/servers.json](https://www.ecoledirecte.com/EDCluster/servers.json)

Échantillon du fichier :
```js
{
  "servers": [ //On va recevoir une liste d'objets comme celui ci dessous
    {
      "profile": "API", //string | Alterne enre API / APIP et ALL
      "lastCheck": "Mon Mar 16 2020 20:55:47 GMT+0100 (CET)", // ???
      "name": "api.ecoledirecte.com", //string | url du serveur
      "ip": "213.32.36.118", //string | ip du serveur
      "status": 1,//int | ???
      "responseTime": 338.23671800643206, //float | ping
      "weigth": 3, //int | ???
      "timeout": false, //bool | Si le serveur a timeout
      "version": "Maj du 16/03/2020 à 16h27", //string, version logicielle du serveur
      "bdTested": "95", //string | ???
      "elapsedTime": 338, //int | ???
      "timingPhases": { //Object representatant la latence avec des détails
        "wait": 78.6058100014925,
        "dns": 3.143696002662182,
        "tcp": 7.182815000414848,
        "firstByte": 249.2012169957161,
        "download": 0.10318000614643097,
        "total": 338.23671800643206
      },
      "timings": {
        "socket": 78.6058100014925,
        "lookup": 81.74950600415468,
        "connect": 88.93232100456953,
        "response": 338.1335380002856,
        "end": 338.23671800643206
      },
      "httptested": "HTTP76",
      "nbConnexions": 1,//int | Nombre d'utilisateur connectés ??
      "errorCode": "ESOCKETTIMEDOUT"
    },
  ],
}
```

----

Ici se trouve toute la documentation, assurez-vous d'avoir lu la référence avant de commencer

## Login

__POST__ `/v3/login.awp`

Data en body :
```json
{
  "identifiant": "Username",
  "motdepasse": "Password",
  "isRelogin": false,
  "uuid": ""
}
```

Différents exemples de réponses complètes. Elles suivent le modèle de réponse donné dans la référence.

<details>
<summary>Succès</summary>

```js
{
  "code": 200,
  "token": "Super secret token here", //Token de connexion (+3100 long)
  "message": "", //Utilisé pour afficher des messages d'erreur
  "data": {
    "accounts": [
      // Liste d'utilisateur liés au compte, voir la référence pour plus
    ]
  }
}
```
</details>

<details>
<summary>Erreur (Mot de passe invalide)</summary>

```js
{
  "code": 505,
  "token": "", //Toujours vide en cas d'erreur
  "message": "Mot de passe invalide !",
  "data": {
    "accounts": []
  }
}
```
</details>

<details>
<summary>Erreur (Utilisateur introuvable)</summary>

```js
{
  "code": 505,
  "token": "", //Toujours vide en cas d'erreur
  "message": "Identifiant et/ou mot de passe invalide !",
  "data": {
    "accounts": []
  }
}
```
</details>


### Accounts objects

Voici la structure détaillée d'un utilisateur (et commentée) qui est contenu sous la clé `data` de la réponse.

```js
{
  "idLogin": 1324, //int | Paramètre de cookie pour savoir si l'utilisateur est connecté via login/password
  "id": 1234,  //int | Numéro de compte
  "uid": "00000000000000000000000000000000", //string | Il ne sert à rien lui aussi
  "identifiant": "Username", //string | Username du compte
  "typeCompte": "E", //string | Voir Type de compte dans la référence
  "codeOgec": "000000000", //string | Code RNE de l'établissement scolaire du compte
  "main": true, //bool | Indique si ce compte est le compte principal de la famille(il peut en avoir plusieur comme avec un parent)
  "lastConnexion": "2021-10-05 18:28", //string | Date de dernière connexion à l'api via l'endpoint login (multiples connexions possibles)
  "civilite": "",
  "prenom": "John",
  "particule": "",
  "nom": "DOE",
  "email": "email@example.com",
  "anneeScolaireCourante": "2050-2051",
  "nomEtablissement": "Github",
  "logoEtablissement": "",
  "couleurAgendaEtablissement": "#1796b0",
  "dicoEnLigneLeRobert": false,
  "socketToken": "secret token", //string | Websocket, à priori il n'est pas prit en compte concernant l'authentification (si on le change on ne sera pas déconnecté)
  "modules": [
    // Liste des modules
  ],
  "parametresIndividuels": {
    "lsuPoilDansLaMainBorne1": "", // int | Paramètre parcoursup
    "lsuPoilDansLaMainBorne2": "", // int | Paramètre parcoursup
    "lsuPoilDansLaMainBorne3": "", // int | Paramètre parcoursup
    "modeCalculLSU": "", // ??? | Paramètre de recommandation parcoursup
    "isQrcode": false, // ???
    "accessibilitéVisuelle": false, // bool | Pour les personnes malvoyantes, une police spéciale est mise en place
    "typeSaisieNotesDefaut": "",
    "nbJoursMaxRenduDevoirCDT": "",
    "typeViewCDTDefaut": ""
  },
  "profile": {
    "sexe": "M",
    "infoEDT": "",
    "nomEtablissement": "Github",
    "idEtablissement": "0",
    "rneEtablissement": "0000000",
    "telPortable": "",
    "idReelEtab": "0",
    "photo": "//doc1.ecoledirecte.com/PhotoEleves/x/y.jpg",
    "classe": {
      "id": 44,
      "code": "TB",
      "libelle": "Terminale B",
      "estNote": 1
    }
  }
}
```

### Account modules

Ces modules là sont ceux disponibles dans l'object account, ils ressemblent à ça :

```js
{
  "code": "CANTINE_BARCODE", //string | Nom du module
  "enable": false, //bool | Si le module est actif pour l'account en question
  "ordre": 1, //int | Position dans la sidebar
  "badge": 0, //int | ???
  "params": {} //object | Parametres du modules
}
```

Voici la liste des modules qui ont été documentés jusqu'a présent

```js
{
  "code": "CANTINE_BARCODE",
  "enable": false,
  "ordre": 1,
  "badge": 0,
  "params": {}
},
{
  "code": "VIE_SCOLAIRE",
  "enable": true,
  "ordre": 10,
  "badge": 0,
  "params": {}
},
{
  "code": "VIE_DE_LA_CLASSE",
  "enable": true,
  "ordre": 20,
  "badge": 0,
  "params": {}
},
{
  "code": "NOTES",
  "enable": true,
  "ordre": 30,
  "badge": 0,
  "params": {}
},
{
  "code": "CLOUD",
  "enable": false,
  "ordre": 90,
  "badge": 0,
  "params": {}
},
{
  "code": "MESSAGERIE",
  "enable": true,
  "ordre": 40,
  "badge": 0,
  "params": {
    "isActif": "1",
    "canParentsLireMessagesEnfants": "1",
    "destAdmin": "0",
    "destEleve": "0",
    "destFamille": "0",
    "destProf": "1",
    "destEspTravail": "0",
    "disabledNotification": "0",
    "notificationEmailEtablissement": "1",
    "choixMailNotification": "0",
    "autreMailNotification": "",
    "mailPro": "",
    "mailPerso": "",
    "messagerieApiVersion": "v3",
    "blackListProfActive": "0",
    "estEnBlackList": "0",
    "afficherToutesLesClasses": "0"
  }
},
{
  "code": "EDT",
  "enable": true,
  "ordre": 50,
  "badge": 0,
  "params": {}
},
{
  "code": "DOCUMENTS_ELEVE",
  "enable": true,
  "ordre": 71,
  "badge": 0,
  "params": {
    "DocumentsNotesActif": "1",
    "DocumentsVSActif": "1",
    "DocumentsAdministratifActif": "1"
  }
},
{
  "code": "CAHIER_DE_TEXTES",
  "enable": true,
  "ordre": 60,
  "badge": 0,
  "params": {
    "compteRenduSeance": "1",
    "compteRenduSeancePrevisionnel": "0",
    "isCDTPrimaire": "0"
  }
},
{
  "code": "MANUELS_SCOLAIRES",
  "enable": false,
  "ordre": 62,
  "badge": 0,
  "params": {}
},
{
  "code": "QCM",
  "enable": true,
  "ordre": 65,
  "badge": 0,
  "params": {}
},
{
  "code": "RESERVATIONS",
  "enable": false,
  "ordre": 80,
  "badge": 0,
  "params": {
    "regime": "Externe",
    "repasmidi_1": "0",
    "repassoir_1": "0",
    "repasmidi_2": "0",
    "repassoir_2": "0",
    "repasmidi_3": "0",
    "repassoir_3": "0",
    "repasmidi_4": "0",
    "repassoir_4": "0",
    "repasmidi_5": "0",
    "repassoir_5": "0",
    "repasmidi_6": "0",
    "repassoir_6": "0",
    "repasmidi_7": "0",
    "repassoir_7": "0"
  }
},
{
  "code": "COMMANDE_PASSAGE",
  "enable": false,
  "ordre": 81,
  "badge": 0,
  "params": {}
},
{
  "code": "CARNET_CORRESPONDANCE",
  "enable": false,
  "ordre": 70,
  "badge": 0,
  "params": {}
},
{
  "code": "ESIDOC",
  "enable": false,
  "ordre": 110,
  "badge": 0,
  "params": {}
},
{
  "code": "EDUNAO",
  "enable": false,
  "ordre": 100,
  "badge": 0,
  "params": {}
},
{
  "code": "CATER",
  "enable": false,
  "ordre": 90,
  "badge": 0,
  "params": {}
},
{
  "code": "ARD",
  "enable": false,
  "ordre": 120,
  "badge": 0,
  "params": {}
},
{
  "code": "PEARLTREES",
  "enable": false,
  "ordre": 170,
  "badge": 0,
  "params": {}
},
{
  "code": "EDUMALIN",
  "enable": false,
  "ordre": 171,
  "badge": 0,
  "params": {}
},
{
  "code": "SUIVI_STAGE",
  "enable": false,
  "ordre": 43,
  "badge": 0,
  "params": {}
},
{
  "code": "CLICKNPLAY",
  "enable": false,
  "ordre": 171,
  "badge": 0,
  "params": {}
},
{
  "code": "VOLTAIRE",
  "enable": false,
  "ordre": 175,
  "badge": 0,
  "params": {}
},
{
  "code": "ONISEPSERVICES",
  "enable": false,
  "ordre": 172,
  "badge": 0,
  "params": {}
},
{
  "code": "AVENRIA",
  "enable": false,
  "ordre": 173,
  "badge": 0,
  "params": {}
},
{
  "code": "SACOCHE",
  "enable": false,
  "ordre": 174,
  "badge": 0,
  "params": {}
},
{
  "code": "ETUDIANT",
  "enable": false,
  "ordre": 176,
  "badge": 0,
  "params": {}
},
{
  "code": "IJBOX",
  "enable": false,
  "ordre": 177,
  "badge": 0,
  "params": {}
},
{
  "code": "FUTURNESS",
  "enable": false,
  "ordre": 178,
  "badge": 0,
  "params": {}
},
{
  "code": "IMPALA",
  "enable": false,
  "ordre": 179,
  "badge": 0,
  "params": {}
},
{
  "code": "SITUATION_FINANCIERE",
  "enable": false,
  "ordre": 92,
  "badge": 0,
  "params": {}
}
```


## Élève

### Timeline

Je sais pas qui aurait besoin de la timeline mais bon (:

__GET__ `/v3/eleves/{id}/timeline.awp`

Data dans la réponse :
```js
[ //Liste des évènements relatif au compte (quelques exemples d'évènement peuvent être trouvés)
  {
    "date": "2021-12-05", //String | Date de l'evènement
    "typeElement": "Note", //String | Type de l'event (Note / Abscence / Document / Messagerie / VieScolaire / ...)
    "idElement": 0, // identifiant du message / evt de vie scolaire / document associé
    "titre": "Nouvelle évaluation", //String | Titre de l'evenement
    "soustitre": "ED.PHYSIQUE & SPORT.",
    "contenu": "natation du 15/11/2021" //String | Contenu de l'evenement
  },
  {
    "date": "2021-09-20",
    "typeElement": "VieScolaire",
    "idElement": 527,
    "titre": "Absence",
    "soustitre": "2 demi-journées",
    "contenu": "Justifiée"
  },
  {
    "date": "2021-09-17",
    "typeElement": "Document",
    "idElement": 2538,
    "titre": "Nouveau document à télécharger",
    "soustitre": "",
    "contenu": "Invitation 50 ans"
  },
  {
    "date": "2021-09-09",
    "typeElement": "Document",
    "idElement": 1144,
    "titre": "Nouveau document à télécharger",
    "soustitre": "",
    "contenu": "Certificat de scolarité"
  }
]
```

### Timeline commune

__GET__ `/v3/E/{id}/timelineAccueilCommun.awp`

C'est la timeline de l'établissement que tous les élèves voient. Elle inclue les post-its.

Data dans la réponse :
```jsonc
{
  "evenements": [], // ?
  "postits": Array<{
    "id": int,
    "type": string, // "info", ?
    "contenu": string // HTML encodé en base64
    "dateCreation": "JJ/MM/AAAA",
    "dateDebut": "JJ/MM/AAAA",
    "dateFin": "JJ/MM/AAAA",
    "cible": [], // ?
    "ciblesEtab": [], // ?
    "auteur": {
      "civilite": string, // M.
      "prenom": string,
      "particule": string,
      "nom": string,
      "type": string, // "A", ?
      "id": string, // Identifiant de la personne
    },
  }>,
}
```

### Sondages

__GET__ `/v3/rdt/sondages.awp`

J'ai eu une réponse avec une liste vide dans `data` donc réponse inconnue.

### Formulaires

__LIST__ `/v3/edforms.awp`

Ici aussi, une liste vide donc réponse inconnue.

### Visios

__GET__ `/v3/E/{id}/visios.awp`

Pas de visios ici non plus donc réponse inconnue.

### Emploi du temps

Le titre est plutot clair non ?

*Note : Si jamais le temps est incorrect alors data sera juste un array vide*

__GET__ `/v3/E/{id}/emploidutemps.awp`

Data en body :
```json
{
  "dateDebut": "2021-12-15",
  "dateFin": "2021-12-15",
  "avecTrous": false
}
```

Data dans la réponse :
```js
[
  {
    "id": 46234, //int | Semble être un identifiant unique du cours
    "text": "NUMERIQUE SC.INFORM.", //string | Nom du cours (peut diférer de la matière)
    "matiere": "NUMERIQUE SC.INFORM.",
    "codeMatiere": "NSINF", //string | Code matière interne au service ED
    "typeCours": "COURS", //string | Semble varier (COURS / PERMANENCE)
    "start_date": "2021-12-15 08:00",
    "end_date": "2021-12-15 08:55",
    "color": "#91b2bc", //string | Couleur hex du cours sur l'edt
    "dispensable": false, //string | Si l'élève doit se rendre en cours
    "dispense": 0, //int | Rhalala, le sport (:
    "prof": "MaitreRouge",
    "salle": "SALLE 11 INFO",
    "classe": "", //string | vide si est dispensé dans plusieures classes
    "classeId": 0, //int | ID de la classe dans laquelle le cours est dispensé (0 si est dispensé dans plusieures classes)
    "classeCode": "", //int | Code de la classe (je pense qu'on a compris avec le temps que c'était vide si la cours était dispensé dans plusieures classes)
    "groupe": "T_NSINF-1", //string | Nom du groupe (si dispensé dans plusieures classes)
    "groupeCode": "T_NSINF-1",//string | Code du groupe
    "isFlexible": false, //bool | ???
    "groupeId": 2004, //int | ID du groupe
    "icone": "", //string | Depuis quand on peut avoir des icones ?
    "isModifie": false, //bool | ???
    "contenuDeSeance": false, //bool | false même si du contenu est posté
    "devoirAFaire": false, //bool | false même si un devoir est posté
    "isAnnule": false //bool | Si le cours est annulé (franglais dégeu)
  },
  {
    "id": 78905,
    "text": "ANGLAIS LV1",
    "matiere": "ANGLAIS LV1",
    "codeMatiere": "AGL1",
    "typeCours": "COURS",
    "start_date": "2021-12-15 10:00",
    "end_date": "2021-12-15 10:55",
    "color": "#ff66cc",
    "dispensable": false,
    "dispense": 0,
    "prof": "Florian L",
    "salle": "SALLE 44",
    "classe": "Terminale B",
    "classeId": 44,
    "classeCode": "TB",
    "groupe": "",
    "groupeCode": "",
    "isFlexible": false,
    "groupeId": 0,
    "icone": "",
    "isModifie": false,
    "contenuDeSeance": false,
    "devoirAFaire": false,
    "isAnnule": false
  }
]
```

### Cahier de texte

__GET__ `/v3/Eleves/{id}/cahierdetexte.awp`

Donne les devoirs à faire à partir d'aujourd'hui (onglet *à faire*).

Data dans la réponse :
```typescript
{
  "AAAA-MM-JJ": Array<{
    matiere: string, // Nom d'affichage ("FRANCAIS" / "ENSEIGN.SCIENTIFIQUE")
    codeMatiere: string, // Code ("FRANC" / "G-SCI")
    aFaire: true, // ?
    idDevoir: number,
    documentsAFaire: false, // ?
    donneLe: "AAAA-MM-JJ",
    effectue: bool, // Si marqué par l'élève comme tel ?
    interrogation: bool,
    rendreEnLigne: bool, // Si présence d'un formulaire pour rendre un fichier ?
  }>,
}
```

__GET__ `/v3/Eleves/{id}/cahierdetexte/{AAAA-MM-JJ}.awp`

Permet d'obtenir le travail à faire en détail et le contenu de séance pour un jour spécifique.

**Note**: Il y a deux champs contenu de séance, un directement dans la matière et un dans à faire. Je suspecte que celui  dans à faire soit prévu avant la séance alors que celui directement dans la matière soit renseigné après les faits.

Je vérifierai la différence quand EcoleDirecte sera à nouveau fonctionnel et que j'aurai un exemple des deux.

Data dans la réponse :
```typescript
{
  date: "AAAA-MM-JJ", // Même date que celle passée dans l'URL
  matieres: Array<{
    entityCode: string, // Classe ("1A") / groupe
    entityLibelle: string, // Nom d'affichage de la classe ("PREMIERE A")
    entityType: "C" | "G" | string, // C = Classe, G = Groupe, ...
    matiere: string,
    codeMatiere: string, // Pareils que route précédente
    nomProf: string,
    id: number, // Même id que idDevoir dans la route précédente
    interrogation: bool,
    blogActif: false, // ?
    nbJourMaxRenduDevoir: number, // Date butoir du rendu ?
    aFaire: {
      idDevoir: number,
      contenu: string, // Détail du travail à faire
      rendreEnLigne: bool,
      donneLe: "AAAA-MM-JJ",
      effectue: bool,
      ressource: "", // ?, vide jusqu'ici
      ressourceDocuments: [], // ?, vide jusqu'ici
      documents: Array<{ // Pièces jointes
        id: number, // Pour la télécharger via la route de téléchargement
        libelle: string, // Nom
        date: "AAAA-MM-JJ",
        taille: number, // En octets
        type: "FICHIER_CDT", // Autres valeurs ?
        signatureDemandee: false, // ?
        signature: {}, // ?
      }>,
      commentaires: Array<{
        id: number, // identifiant du commentaire
        idAuteur: number, // identifiant de l'auteur
        profilAuteur: "E", // E = Elève, autres valeurs ?
        auteur: string, // Nom de l'auteur
        date: "AAAA-MM-JJ",
        message: string, // Encodé en base64
        supprime: false, // Si le commentaire a été supprimé ? (semble stupide)
      }>,
      elementsProg: [], // ?
      liensManuel: [], // URL des manuels associés à la matière ?
      documentsRendus: [], // Fichiers rendus lorsque le formulaire est présent ?
      contenuDeSeance: {
        contenu: string,
        documents: [],
        commentaires: [],
      },
    },
    contenuDeSeance: {
      idDevoir: number, // Systématiquement le même identifiant que id plus haut
      contenu: string,
      documents: [], // Pièces jointes ?
      commentaires: [], // Même structure que commentaires sur travail à faire ?
      elementsProg: [],
      liensManuel: [],
    },
  }>
}
```

__PUT__ `/v3/Eleves/{id}/cahierdetexte.awp`

Marque des devoirs comme faits ou non faits.

Data en body :
```typescript
{
  idDevoirsEffectues: number[],
  idDevoirsNonEffectues: number[],
}
```

__POST__ `/v3/eleves/{id}/afaire/commentaires.awp`

Poste un commentaire sous un travail à faire ou un contenu de séance

Data en body :
```typescript
{
  message: string, // encodé en base64
  idContenu: number, // identifiant du devoir / contenu de séance
}
```

Data dans la réponse :
```typescript
{
  id: number, // Identifiant du commentaire posté
}
```

### Vie scolaire

Endpoint qui sert a afficher les données sur la page vie de classe

__GET__ `/v3/eleves/{id}/viescolaire.awp`

Data dans la réponse :
```js
{
  "absencesRetards":[
    {
      "id":2936, //int | Identifiant interne de l'abscence / retard
      "idEleve":0, //int | Normalement égal à l'id de l'eleve mais 0 dans mon cas
      "nomEleve":"", //string | Normalement rempli mais vide dans mon cas
      "typeElement":"Absence", //string | Sert pour savoir si c'est une abscence ou un retard
      "date":"2021-11-19",
      "displayDate":"le vendredi 19 novembre 2021 de 08:30 à 16:30",
      "libelle":"2 demi-journées",
      "motif":"",
      "justifie":true, //bool | Si jamais justufié ou non (note : si jamais elle est en attente de justification, elle sera a true)
      "par":"",
      "commentaire":"Merci de bien vouloir excuser l'absence de X pour la journée, il est souffrant.\nBien cordialement,",
      "typeJustification":" en attente de prise en compte par la vie scolaire",
      "justifieEd":true,
      "aFaire":"",
      "dateDeroulement":""
    }
  ],
  "sanctionsEncouragements":[
    {
      "id":48,
      "idEleve":0,
      "nomEleve":"",
      "typeElement":"Punition",
      "date":"2021-11-26",
      "displayDate":"",
      "libelle":"RETENUE",
      "motif":"Trop de mots dans le carnet",
      "justifie":false,
      "par":"",
      "commentaire":"",
      "typeJustification":"",
      "justifieEd":false,
      "aFaire":"1 heure",
      "dateDeroulement":"S'est déroulé le mer. 08 décembre<BR>de 13:00 à 14:00"
    }
  ],
  "parametrage": {
    "justificationEnLigne":true,
    "absenceCommentaire":true,
    "retardCommentaire":true,
    "sanctionsVisible":true,
    "sanctionParQui":false,
    "sanctionCommentaire":true,
    "encouragementsVisible":false,
    "encouragementParQui":false,
    "encouragementCommentaire":false
  }
}
```

### Carnet de correspondance

Permet d'obtenir une liste des correspondances.

__GET__ `/v3/eleves/{id}/eleveCarnetCorrespondance.awp`

Data dans la réponse :
```jsonc
{
  "correspondances": [
    // ?
  ],
  "suivis": [
    // ?
  ]
}
```

### Documents administratifs

Permet d'obtenir la liste des documents administratifs associées à une année scolaire, ou à l'année actuelle.

__GET__ `/v3/elevesDocuments.awp`

Paramètres de recherche :
 - `archive=` pour l'année actuelle
 - `archive=AAAA-AAAA` pour spécifier une année précise

Data dans la réponse :
```typescript
{
  factures: [], // ?
  notes: Array<Document>,
  viescolaire: [], // ?
  administratifs: Array<Document>,
  inscriptions?: [], // ?
  listesPiecesAVerser: {
    listePieces: [], // ?
    personnes: Array<{id: 1234, nom: "…", prenom: "…", type: "E"}>,
    pieces: [], // ?
    televersements: [] // ?
  }
}
```

Type des objets document :
```typescript
type Document = {
  id: int,
  libelle: string,
  idEleve: int,
  date: "AAAA-MM-JJ",
  type: "Note" | "Doc" | "",
  signatureDemandee: bool, // ?
  signature: {}
}
```

Les documents peuvent être téléchargés via la route de téléchargement grâce à leur id.
