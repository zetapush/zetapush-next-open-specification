# Objectifs

* Un développeur doit pouvoir appeler un _cloud service_ (ZetaPush ou custom) et avoir une réponse en retour.
* Un développeur doit pouvoir développer son code métier dans des _custom cloud services_.
* Un développeur doit pouvoir utiliser l'autocomplétion de son IDE pour l'aider dans ses appels aux _cloud services_.
* Un développeur doit avoir accès depuis son IDE à la documentation des _cloud services_.
* Un développeur doit pouvoir écouter les évènements des _cloud services_.
* Un développeur doit pouvoir exécuter ses _custom cloud services_ en local

Les profils ainsi que le vocabulaire utilisés sont définis dans [le readme](./README.md#vocabulaire).



# Pré-requis

* J'ai un compte sur ZetaPush
* J'ai une application front qui est prête à utiliser les _cloud services_. (Projet initialisé, dépendances installées, éventuellement la configuration de saisie)
* Nous utilisons le **Avengers Chat** comme cas d'usage (Chat temps réel)


# <a name="parcours-1"></a> ![Parcours 1](https://img.shields.io/badge/parcours-dev%20front-00d0ff.svg) : Je développe sur mon environnement local une application front avec ZetaPush sans _custom cloud service_

## Schéma

![Développement avec services ZetaPush + Service custom](https://exp.draw.io/ImageExport4/export?url=https://raw.githubusercontent.com/zetapush/zetapush-next-open-specification/master/schemas/archi-dev-front-with-custom-service.html)


## User Stories


### <a name="P01-DEV01"></a> [P01-DEV01] ETQ dev front j'utilise un _cloud service_ et j'ai une réponse en retour 

![celtia-alpha-3](https://img.shields.io/badge/milestone-celtia--alpha--3-blue.svg)

*GIVEN*

* Je suis en cours de développement de mon _Avengers Chat_
* J'ai un compte ZetaPush (`user@gmail.com`/`password`)
* J'ai précisé mon nom d'organisation (`mon-orga-a-moi`)
* Mon application _Avengers Chat_ est déclarée sur ZetaPush
* Mon application dispose de deux environnements dont les nom sont `dev` et `prod`
* J'ai configuré le SDK client ZetaPush pour pointer sur mon application et mon environnement :
  ```javascript
  const client = new ZetaPush.SmartClient({
    appName: 'Avengers Chat',
    organizationName: 'mon-orga-a-moi',
    env: 'dev'
  });
  ```
* J'ai importé le _cloud service_ `UserService` qui me permet de gérer des utilisateurs
* J'ai accès à `UserService` via l'objet `userService`
* J'ai une _cloud function_ qui me permet de créer des utilisateurs : `createUser()`
* Je souhaite créer un utilisateur depuis mon code front
* J'écris le code suivant :

  ```javascript
  /**
   * I call the "createUser()" cloud function with many parameters.
   * "userService" is a UserService
   */
  this.userService.createUser({
    firstname: "Peter",
    lastname: "Parker",
    username: "Spider-Man"
  })
  .then((newUser) => {
    console.log(`new user created : ${newUser.username}`);
  })
  .catch((error) => {
    console.error(`Error to create new user : ${error.message}`);
  });


  /**
   *  Other syntaxe to call a cloud function
   */
  const newUser = await this.userService.createUser({
    firstname: "Peter",
    lastname: "Parker",
    username: "Spider-Man"
  });
  ```

*WHEN*

* Lorsque j'appelle ma _cloud function_ `createUser` via le _cloud service_ `UserService` et que tout s'est bien passé

*THEN*

* L'appel au service ZetaPush se fait de manière asynchrone et une réponse est renvoyée
* La réponse renvoyée est sous la forme d'une promesse

---

### <a name="P01-DEV02"></a> [P01-DEV02] ETQ dev front j'utilise un _cloud service_ et j'ai une erreur en retour 

![celtia-alpha-3](https://img.shields.io/badge/milestone-celtia--alpha--3-blue.svg)

*GIVEN*

* Je suis en cours de développement de mon _Avengers Chat_
* J'ai un compte ZetaPush (`user@gmail.com`/`password`)
* J'ai précisé mon nom d'organisation (`mon-orga-a-moi`)
* Mon application _Avengers Chat_ est déclarée sur ZetaPush
* Mon application dispose de deux environnements dont les nom sont `dev` et `prod`
* J'ai configuré le SDK client ZetaPush pour pointer sur mon application et mon environnement :
  ```javascript
  const client = new ZetaPush.SmartClient({
    appName: 'Avengers Chat',
    organizationName: 'mon-orga-a-moi',
    env: 'dev'
  });
  ```
* J'ai importé le _cloud service_ `UserService` qui me permet de gérer des utilisateurs
* J'ai accès à `UserService` via l'objet `userService`
* J'ai une _cloud function_ qui me permet de créer des utilisateurs : `createUser()`
* Je souhaite créer un utilisateur depuis mon code front
* J'écris le code suivant :

  ```javascript
  /**
   * I call the "createUser()" cloud function with many parameters.
   * "userService" is a UserService
   */
  this.userService.createUser({
    firstname: "Peter",
    lastname: "Parker",
    username: "Spider-Man"
  })
  .then((newUser) => {
    console.log(`new user created : ${newUser.username}`);
  })
  .catch((error) => {
    console.error(`Error to create new user : ${error.message}`);
  });


  /**
   *  Other syntaxe to call a cloud function
   */
  const newUser = await this.userService.createUser({
    firstname: "Peter",
    lastname: "Parker",
    username: "Spider-Man"
  });
  ```

*WHEN*

* Lorsque j'appelle ma _cloud function_ `createUser` via le _cloud service_ `UserService` et qu'une erreur est survenue

*THEN*

* L'appel au service ZetaPush se fait de manière asynchrone et une réponse est renvoyée
* La réponse renvoyée est sous la forme d'une promesse
* Comme il y a eu un dysfonctionnement, une erreur est renvoyée dans la partie `.catch()`
* L'objet d'erreur renvoyé comporte 2 paramètres : `tag` qui est un identifiant unique pour cette erreur et `message` qui est une description plus précise de cette erreur.


---


### <a name="P01-DEV03"></a> [P01-DEV03] ETQ dev front j'écoute les évènements d'un _cloud service_

![celtia-alpha-3](https://img.shields.io/badge/milestone-celtia--alpha--3-blue.svg)

*GIVEN*


* Je suis en cours de développement de mon _Avengers Chat_
* J'ai un compte ZetaPush (`user@gmail.com`/`password`)
* J'ai précisé mon nom d'organisation (`mon-orga-a-moi`)
* Mon application _Avengers Chat_ est déclarée sur ZetaPush
* Mon application dispose de deux environnements dont les nom sont `dev` et `prod`
* J'ai configuré le SDK client ZetaPush pour pointer sur mon application et mon environnement :
  ```javascript
  const client = new ZetaPush.SmartClient({
    appName: 'Avengers Chat',
    organizationName: 'mon-orga-a-moi',
    env: 'dev'
  });
  ```
* J'ai importé le _cloud service_ `UserService` qui me permet de gérer des utilisateurs
* J'ai accès à `UserService` via l'objet `userService`
* J'ai une _cloud function_ qui me permet de créer des utilisateurs : `createUser()`
* Je souhaite écouter l'évènement de la création d'utilisateur
* J'écris le code suivant pour écouter les évènements de création d'utilisateurs :

```javascript
this.userService.onCreateUser = user => {
  console.log(`My new Avenger is : ${user.username}`);
};
```

*WHEN*

* Lorsqu'un nouvel utilisateur est créé sur mon application

*THEN*

* L'évènement `onCreateUser` est appelé et l'action résultante est exécutée
* Dans notre exemple, `My new Avenger is : "name"` est affiché dans sortie console

#### Précisions

Afin d'écouter les évènements d'une _cloud function_ précise, il faut écouter l'évènement suivant la syntaxe suivante :

* "on" + _nameCloudFunction_ en camelCase

Les évènements sont envoyés seulement à l'utilisateur qui appelle la _cloud function_.

---

### <a name="P01-DEV04"></a> [P01-DEV04] ETQ dev front j'utilise l'autocompletion de mon IDE (VSCode) pour découvrir et utiliser les _cloud services_

![celtia-alpha-3](https://img.shields.io/badge/milestone-celtia--alpha--3-blue.svg)


*GIVEN*

* J'utilise Visual Studio Code comme IDE
* Je suis en cours de développement de mon _Avengers Chat_
* J'ai importé le _cloud service_ `UserService` qui me permet de gérer des utilisateurs
* J'ai accès à `UserService` via l'objet `userService`
* J'ai différentes _cloud functions_ disponibles au sein de mon _cloud service_ : `createUser()` / `createOrganization()` / `getUser()`

*WHEN*

* Lorsque je commence à écrire mon appel au _cloud service_ : `this.userService.create`

*THEN*

* L'autocompletion me propose les différentes _cloud functions_ auxquelles j'ai accès en fonction de ma saisie :
  * `this.userService.createUser()`
  * `this.userService.createOrganization()`
* La _cloud function_ `getUser()` ne correspond pas et n'est donc pas affichée


---


### <a name="P01-DEV05"></a> [P01-DEV05] ETQ dev front j'ai accès à la documentation des _cloud services_ depuis mon IDE (VSCode)

![celtia-beta-1](https://img.shields.io/badge/milestone-celtia--beta--1-blue.svg)

*GIVEN*

* J'utilise Visual Studio Code comme IDE
* Je suis en cours de développement de mon _Avengers Chat_
* J'ai importé le _cloud service_ `UserService` qui me permet de gérer des utilisateurs
* J'ai accès à `UserService` via l'objet `userService`
* J'ai différentes _cloud functions_ disponibles au sein de mon _cloud service_ : `createUser()` / `createOrganization()` / `getUser()`

*WHEN*

* Lorsque je commence à écrire mon appel au _cloud service_ : `this.userService.create`

*THEN*

* L'autocomplétion s'affiche pour me proposer les différentes _cloud functions_ auxquelles j'ai accès en fonction de ma saisie
* Au sein de la pop-up d'autocomplétion, je peux cliquer sur un lien `view documentation` qui va me rediriger (Toujours dans la pop-up) sur la liste des _cloud functions_ pour ce _cloud service_
* Je peux naviguer au sein de la documentation dans la pop-up, en utilisant différents liens.
* Pour chaque _cloud function_ il y a son nom, sa description, ses paramètres entrants et son retour


---

### <a name="P01-DEV06"></a> [P01-DEV06] ETQ dev front j'utilise un _cloud service_ sans renseigner mes informations de connexion à ZetaPush

![celtia-alpha-2](https://img.shields.io/badge/milestone-celtia--alpha--2-blue.svg)

*GIVEN*

* Je suis en cours de développement de mon _Avengers Chat_
* Je n'ai pas de compte ZetaPush
* J'ai importé le _cloud service_ `UserService` qui me permet de gérer des utilisateurs
* J'ai accès à `UserService` via l'objet `userService`
* J'ai une _cloud function_ qui me permet de créer des utilisateurs : `createUser()`
* Je souhaite créer un utilisateur depuis mon code front
* J'écris le code suivant :

  ```javascript
  /**
   * I call the "createUser()" cloud function with many parameters.
   * "userService" is a UserService
   */
  this.userService.createUser({
    firstname: "Peter",
    lastname: "Parker",
    username: "Spider-Man"
  })
  .then((newUser) => {
    console.log(`new user created : ${newUser.username}`);
  })
  .catch((error) => {
    console.error(`Error to create new user : ${error.message}`);
  });


  /**
   *  Other syntaxe to call a cloud function
   */
  const newUser = await this.userService.createUser({
    firstname: "Peter",
    lastname: "Parker",
    username: "Spider-Man"
  });
  ```
* Je n'ai pas configuré le SDK client ZetaPush pour pointer sur mon application et mon environnement :
  ```javascript
  const client = new ZetaPush.SmartClient({});
  ```

*WHEN*

* Lorsque j'appelle ma _cloud function_ `createUser` via le _cloud service_ `UserService`

*THEN*

* La console du navigateur affiche une erreur indiquant qu'aucune information ZetaPush n'est précisée:
  ```console
  Missing application information for ZetaPush startup.
  More information here: https://console.zetapush.com/documentation/sdk-js/startup

  You don't have a ZetaPush account yet ?
    - Create a temporary account by executing the command "zeta register". Your application will be automatically registered and you can then use the displayed names for ZetaPush startup.
    - Open ZetaPush console to create an account and register a new application: https://console.zetapush.com/signup

  You already have a ZetaPush account but the application is not registered ?
    - Fill the file .zetarc with your credentials and run "zeta register". Your application will be automatically registered and you can then use the displayed identifier for ZetaPush startup.
    - Create a new application through ZetaPush console: https://console.zetapush.com/app/register 

  You already have a ZetaPush account and the application is registered ?
    - Provide your application name, organisation name and environment name for ZetaPush startup: https://console.zetapush.com/documentation/sdk-js/startup
  ```
* La réponse renvoyée est sous la forme d'une promesse
* La réponse est renvoyée dans le `.then()`


# <a name="parcours-2"></a> ![Parcours 2](https://img.shields.io/badge/parcours-dev%20full--stack-00d0ff.svg) Je développe sur mon environnement local une application avec ZetaPush et des _custom cloud services_

## Schéma

![Développement avec seulement les services ZetaPush](https://exp.draw.io/ImageExport4/export?url=https://raw.githubusercontent.com/zetapush/zetapush-next-open-specification/master/schemas/archi-dev-front-with-custom-service.html)

## User Stories

### <a name="P02-DEV01"></a> [P02-DEV01] ETQ dev full-stack je développe et exécute mon code métier

![celtia-alpha-1](https://img.shields.io/badge/milestone-celtia--alpha--1-blue.svg)

*GIVEN*

* Je suis en cours de développement de mon _Avengers Chat_
* Je souhaite réaliser mon code métier et le déployer côté back
* Je souhaite créer un _custom cloud service_ nommé `AvengersService` qui va me permettre de réaliser le code métier de mon chat
* Dans un premier temps je veux seulement ajouter une _cloud function_ à mon _custom cloud service_ qui est `attackWithRandomSkill()` qui va me permettre de lancer une attaque aléatoire en fonction du personnage (chaque Avenger a plusieurs compétences possibles)
* J'ai écris mon _custom cloud service_ comme ci-dessous : 

  ```javascript
  /**
   *  This class gathers the cloud functions for my own custom cloud service
   *  Each method is a cloud function
   */
  class AvengersService {

    /**
    *  Function to print that an Avenger do a random attack
    */
    attackWithRandomSkill(avengerName) {
      // Get the Avengers
      const avenger = this.userService.getUserByLogin({login: avengerName}):

      // Get a random power of this Avenger
      const superpower = avenger.superpowers[Math.floor(Math.random()*avenger.superpowers.length)];

      // Print that we attack with a random skill !
      print(`${avengerName} attacks with ${superpower} !!!`);

      return superpower;
    }
  }
  ```
 * Je souhaite exécuter mon code back pour le tester

*WHEN*
- J'exécute mon _custom cloud service_ en local avec la commande : `zeta run worker`

*THEN*
- Je peux utiliser mon _custom cloud service_ dans mon code front en appelant les _cloud functions_ sous la forme suivante :


  ```javascript
  /**
   *  "avengersService" is an instance of AvengersService
   */
  this.avengersService.attackWithRandomSkill("Spider-Man");
  ```

- Un listener d'évènement est automatiquement créé pour chaque _cloud function_, ici nous pouvons écouter la création de jeu avec :

  ```javascript
  /**
   *  "avengersService" is an instance of AvengersService
   *  We assume that the attackWithRandomSkill() cloud function return a string
   */
  this.avengersService.onAttackWithRandomSkill = superpower => {
    console.log(`The Avenger attacks with the skill : ${superpower} !!`);
  };
  ```

#### Précisions

Afin d'écouter les évènements d'une _cloud function_ précise, il faut écouter l'évènement suivant la syntaxe suivante :

* "on" + _nameCloudFunction_ en camelCase

Les évènements sont envoyés seulement à l'utilisateur qui appelle la _cloud function_.


---


### <a name="P02-DEV05"></a> [P02-DEV05] ETQ dev full-stack j'utilise l'autocompletion de mon IDE (VSCode) pour découvrir et utiliser mes _custom cloud services_

![celtia-alpha-2](https://img.shields.io/badge/milestone-celtia--alpha--2-blue.svg)

*GIVEN*

* J'utilise Visual Studio Code comme IDE
* Je suis en cours de développement de mon _Avengers Chat_
* J'ai importé le _custom cloud service_ `AvengersService` qui me permet de gérer un jeu
* J'ai accès à `AvengersService` via l'objet `avengersService`
* J'ai différentes _cloud functions_ disponibles au sein de mon _custom cloud service_ : `attackWithRandomSkill()` / `attackWithSpecificSkill()` / `getHealthAvenger()`

*WHEN*

* Lorsque je commence à écrire mon appel au _custom cloud service_ : `this.avengersService.attack`

*THEN*

* L'autocompletion me propose les différentes _cloud functions_ auxquelles j'ai accès en fonction de ma saisie :
  * `this.avengersService.attackWithRandomSkill()`
  * `this.avengersService.attackWithSpecificSkill()`
* La _cloud function_ `getHealthAvenger()` ne correspond pas et n'est donc pas affichée


---


### <a name="P02-DEV06"></a> [P02-DEV06] ETQ dev full-stack j'ai accès à la documentation de mes _custom cloud services_ depuis mon IDE (VSCode)

![celtia-beta-1](https://img.shields.io/badge/milestone-celtia--beta--1-blue.svg)

*GIVEN*

* J'utilise Visual Studio Code comme IDE
* Je suis en cours de développement de mon _Avengers Chat_
* J'ai importé le _custom cloud service_ `AvengersService` qui me permet de gérer un jeu
* J'ai accès à `AvengersService` via l'objet `avengersService`
* J'ai différentes _cloud functions_ disponibles au sein de mon _custom cloud service_ : `attackWithRandomSkill()` / `attackWithSpecificSkill()` / `getHealthAvenger()`

*WHEN*

* Lorsque je commence à écrire mon appel au _custom cloud service_ : `this.avengersService.attack`

*THEN*

* L'autocomplétion s'affiche pour me proposer les différentes _cloud functions_ auxquelles j'ai accès en fonction de ma saisie
* Au sein de la pop-up d'autocomplétion, je peux cliquer sur un lien `view documentation` qui va me rediriger (Toujours dans la pop-up) sur la liste des _cloud functions_ pour ce _cloud service_
* Je peux naviguer au sein de la documentation dans la pop-up, en utilisant différents liens.
* Pour chaque _cloud function_ il y a son nom, sa description, ses paramètres entrants et son retour

### <a name="P02-DEV07"></a> [P02-DEV07] ETQ dev full-stack je développe et exécute mon code métier sans compte ZetaPush

![celtia-alpha-1](https://img.shields.io/badge/milestone-celtia--alpha--1-blue.svg)

*GIVEN*

* Je suis en cours de développement de mon _Avengers Chat_
* Je souhaite réaliser mon code métier et le déployer côté back
* Je souhaite créer un _custom cloud service_ nommé `AvengersService` qui va me permettre de réaliser le code métier de mon chat
* Dans un premier temps je veux seulement ajouter une _cloud function_ à mon _custom cloud service_ qui est `attackWithRandomSkill()` qui va me permettre de lancer une attaque aléatoire en fonction du personnage (chaque Avenger a plusieurs compétences possibles)
* J'ai écris mon _custom cloud service_ comme ci-dessous : 

  ```javascript
  /**
   *  This class gathers the cloud functions for my own custom cloud service
   *  Each method is a cloud function
   */
  class AvengersService {

    /**
    *  Function to print that an Avenger do a random attack
    */
    attackWithRandomSkill(avengerName) {
      // Get the Avengers
      const avenger = this.userService.getUserByLogin({login: avengerName}):

      // Get a random power of this Avenger
      const superpower = avenger.superpowers[Math.floor(Math.random()*avenger.superpowers.length)];

      // Print that we attack with a random skill !
      print(`${avengerName} attacks with ${superpower} !!!`);

      return superpower;
    }
  }
  ```
 * Je souhaite exécuter mon code back pour le tester
 * Je n'ai pas de compte ZetaPush

*WHEN*
- J'exécute mon _custom cloud service_ en local avec la commande `zeta run worker`

*THEN*
- Un compte ZetaPush est créé automatiquement
- Une nouvelle application est créée sur le nouveau compte
- Les différents services et les différents _custom cloud services_ sont déployés sur l'application
- Je peux utiliser mon _custom cloud service_ dans mon code front en appelant les _cloud functions_ sous la forme suivante :


  ```javascript
  /**
   *  "avengersService" is an instance of AvengersService
   */
  this.avengersService.attackWithRandomSkill("Spider-Man");
  ```

- Un listener d'évènement est automatiquement créé pour chaque _cloud function_, ici nous pouvons écouter la création de jeu avec :

  ```javascript
  /**
   *  "avengersService" is an instance of AvengersService
   *  We assume that the attackWithRandomSkill() cloud function return a string
   */
  this.avengersService.onAttackWithRandomSkill = superpower => {
    console.log(`The Avenger attacks with the skill : ${superpower} !!`);
  };
  ```

#### Précisions

Afin d'écouter les évènements d'une _cloud function_ précise, il faut écouter l'évènement suivant la syntaxe suivante :

* "on" + _nameCloudFunction_ en camelCase

Les évènements sont envoyés seulement à l'utilisateur qui appelle la _cloud function_.

---


### <a name="P02-DEV09"></a> [P02-DEV09] ETQ dev full-stack je m'enregistre sur ZetaPush

![celtia-alpha-1](https://img.shields.io/badge/milestone-celtia--alpha--1-blue.svg)

*GIVEN*

* Je n'ai pas de compte sur ZetaPush
* J'ai une application prête à fonctionner avec ZetaPush avec l'arborescence suivante:
  ```
  myApp
  ├── .zetarc
  ├── .gitignore
  ├── front
  │   ├── index.html
  │   └── index.js
  ├── worker
  │   └── index.js
  └── package.json
  ```
* Mon application est nommée `avengers-chat` dans le package.json
* Je développe mon front en local
* Je développe mes _custom cloud services_ en local
* Je souhaite utiliser mon adresse `user@gmail.com` en tant que login et `azertyuiop` en tant que mot de passe

*WHEN*

* J'exécute la commande `zeta register --developer-login user@gmail.com`

*THEN*

* Un prompt me demande de choisir mon mot de passe
* Un prompt me demande de confirmer mon mot de passe
* Mon compte est créé sur ZetaPush
* Mon application `avengers-chat` est enregistrée auprès de ZetaPush
* Je peux retrouver mon application dans la console de ZetaPush (https://console.zetapush.com/apps)
* Le fichier `.zetarc` est mis à jour avec le contenu suivant :
```bash
ZP_DEVELOPER_LOGIN = user@gmail.com
ZP_DEVELOPER_PASSWORD = azertyuiop
```
* Je peux relancer mes _custom cloud services_ en local
* Je peux héberger mon front sur ZetaPush avec mon nouveau compte
* Je peux déployer mes _custom cloud services_ sur ZetaPush avec mon nouveau compte


#### Précisions

Afin d'écouter les évènements d'une _cloud function_ précise, il faut écouter l'évènement suivant la syntaxe suivante :

* "on" + _nameCloudFunction_ en camelCase

Les évènements sont envoyés seulement à l'utilisateur qui appelle la _cloud function_.



---

### <a name="P02-DEV10"></a> [P02-DEV10] ETQ dev full-stack je développe et exécute mon code métier organisé par domaine

![celtia-alpha-1](https://img.shields.io/badge/milestone-celtia--alpha--1-blue.svg)

*GIVEN*

* Je suis en cours de développement de mon application _Avengers Chat_
* Je souhaite réaliser mon code métier et le déployer côté back
* Je souhaite créer un _custom cloud service_ nommé `AvengersUserService` qui va me permettre de réaliser le code métier de gestion des avengers:
  ```javascript
  export class AvengersUserService {
    constructor(zpStorageService, zpUserService, avengersSkillService) {
      this.zpAvengersStorage = zpStorageService.forCollection('avengers')
      this.zpUserService = zpUserService
      this.avengersSkillService = avengersSkillService
    }
    async addAvenger(name, firstname, lastname, skills) {
      let id = ...
      let savedSkills = await this.avengersSkillService.setSkills(id, skills)
      await this.zpAvengersStorage.save({id, name, realIdentity: {firstname, lastname}, skills: savedSkills})
      return await this.zpUserService.addUser({id, name})
    }
    async getAvengerUser(id) {
      return await this.zpUserService.byId(id)
    }
    async getAvengerInfo(id) {
      return await this.zpAvengersStorage.byId(id)
    }
  }
  ```
* Je souhaite créer un _custom cloud service_ nommé `AvengersSkillService` qui va me permettre de réaliser le code métier de gestion des compétences des avengers
  ```javascript
  export class AvengersSkillService {
    constructor(zpStorageService) {
      this.zpSkillsStorage = zpStorageService.forCollection('skills')
    }
    async setSkills(skills) {
      let savedSkills = []
      for(let skill of skills) {
        let id = ...
        savedSkills.push(await this.zpSkillsStorage.save({id, name}))
      }
      return savedSkills
    }
  }
  ```
* Je souhaite créer un _custom cloud service_ nommé `AvengersChatService` qui va me permettre de réaliser le code métier de gestion du chat
  ```javascript
  export class AvengersChatService {
    constructor(zpChatService, avengersUserService, avengersSkillService) {
      this.zpChatService = zpChatService
      this.avengersUserService = avengersUserService
      this.avengersSkillService = avengersSkillService
    }
    async newChat(avengerIds) {
      let room = await zpChatService.createRoom()
      for(let id of avengerIds) {
        let avenger = await this.avengersUserService.getAvengerUser(id)
        room.add(avenger)
      }
      return room
    }
    async talkToEveryone(sender, message, roomId) {
      let room = await zpChatService.getRoomById(roomId)
      return await room.addMessage({from: sender, message})
    }
    async tease(teaser, teased, message, roomId) {
      let room = await zpChatService.getRoomById(roomId)
      return await room.addMessage({from: teaser, message: "@${teased} ${message}"})
    }
    async attack(attacker, attacked, skill, roomId) {
      let room = await zpChatService.getRoomById(roomId)
      return await room.addMessage({from: attacker, message: "@${attacked.name} I attack you using ${skill.name}")
    }
  }
  ```
* Je souhaite que mon _custom cloud service_ `AvengersSkillService` ne soit pas exposé au client
* Je déclare mes _custom cloud service_ pour que ZetaPush puisse les créer et les injecter (dans `index.js`) :
  ```javascript
  import { AvengersUserService, AvengersSkillService, AvengersChatService } from ...

  export default {
    exposed: {
      avengersUserService: AvengersUserService,
      avengersChatService: AvengersChatService
    },
    internal: {
      avengersSkillService: AvengersSkillService
    }
  }
  ```

*WHEN*
- J'exécute mon _custom cloud service_ en local avec la commande : `zeta run worker`

*THEN*
- Je peux utiliser mes _custom cloud services_ `AvengersUserService` et `AvengersChatService` dans mon code front en appelant les _cloud functions_ sous la forme suivante :
  ```javascript
  let superman = await this.avengersUserService.addAvenger("Superman", "Clark", "Kent", [
    {name: "Flight"}, 
    {name: "Superhuman Strength"}, 
    {name: "Superhuman Speed"}, 
    {name: "Superhuman Breath"}, 
    {name: "X-Ray Vision"}, 
    {name: "Superhuman hearing"}, 
    {name: "Superhuman Vision"}])
  let batman = await this.avengersUserService.addAvenger("Batman", "Bruce", "Wayne", [])
  let room = await this.avengersChatService.newChat([spiderman.id, batman.id]);
  this.avengersChatService.tease(superman, batman, "You have no chance", room.id)
  this.avengersChatService.tease(batman, superman, "I have no power but I'm not afraid", room.id)
  ```
- Je ne peux pas utiliser mon _custom cloud service_ `AvengersSkillService` depuis mon front


---

### <a name="P02-DEV11"></a> [P02-DEV10] ETQ dev full-stack je développe et exécute mon code métier organisé par domaine en TypeScript

![celtia-alpha-2](https://img.shields.io/badge/milestone-celtia--alpha--2-blue.svg)

*GIVEN*

* Je suis en cours de développement de mon application _Avengers Chat_
* Je souhaite réaliser mon code métier et le déployer côté back
* Je souhaite créer un _custom cloud service_ nommé `AvengersUserService` qui va me permettre de réaliser le code métier de gestion des avengers:
  ```javascript
  @Exposed({id: 'avengersUserService'})
  export class AvengersUserService {
    constructor(zpStorageService, zpUserService, avengersSkillService) {
      this.zpAvengersStorage = zpStorageService.forCollection('avengers')
      this.zpUserService = zpUserService
      this.avengersSkillService = avengersSkillService
    }
    async addAvenger(name, firstname, lastname, skills) {
      let id = ...
      let savedSkills = await this.avengersSkillService.setSkills(id, skills)
      await this.zpAvengersStorage.save({id, name, realIdentity: {firstname, lastname}, skills: savedSkills})
      return await this.zpUserService.addUser({id, name})
    }
    async getAvengerUser(id) {
      return await this.zpUserService.byId(id)
    }
    async getAvengerInfo(id) {
      return await this.zpAvengersStorage.byId(id)
    }
  }
  ```
* Je souhaite créer un _custom cloud service_ nommé `AvengersSkillService` qui va me permettre de réaliser le code métier de gestion des compétences des avengers
* Je souhaite que mon _custom cloud service_ `AvengersSkillService` ne soit pas exposé au client
  ```javascript
  @Internal({id: 'avengersSkillService'})
  export class AvengersSkillService {
    constructor(zpStorageService) {
      this.zpSkillsStorage = zpStorageService.forCollection('skills')
    }
    async setSkills(skills) {
      let savedSkills = []
      for(let skill of skills) {
        let id = ...
        savedSkills.push(await this.zpSkillsStorage.save({id, name}))
      }
      return savedSkills
    }
  }
  ```
* Je souhaite créer un _custom cloud service_ nommé `AvengersChatService` qui va me permettre de réaliser le code métier de gestion du chat
  ```javascript
  @Exposed({id: 'avengersChatService'})
  export class AvengersChatService {
    constructor(zpChatService, avengersUserService, avengersSkillService) {
      this.zpChatService = zpChatService
      this.avengersUserService = avengersUserService
      this.avengersSkillService = avengersSkillService
    }
    async newChat(avengerIds) {
      let room = await zpChatService.createRoom()
      for(let id of avengerIds) {
        let avenger = await this.avengersUserService.getAvengerUser(id)
        room.add(avenger)
      }
      return room
    }
    async talkToEveryone(sender, message, roomId) {
      let room = await zpChatService.getRoomById(roomId)
      return await room.addMessage({from: sender, message})
    }
    async tease(teaser, teased, message, roomId) {
      let room = await zpChatService.getRoomById(roomId)
      return await room.addMessage({from: teaser, message: "@${teased} ${message}"})
    }
    async attack(attacker, attacked, skill, roomId) {
      let room = await zpChatService.getRoomById(roomId)
      return await room.addMessage({from: attacker, message: "@${attacked.name} I attack you using ${skill.name}")
    }
  }
  ```

*WHEN*
- J'exécute mon _custom cloud service_ en local avec la commande : `zeta run worker`

*THEN*
- Je peux utiliser mes _custom cloud services_ `AvengersUserService` et `AvengersChatService` dans mon code front en appelant les _cloud functions_ sous la forme suivante :
  ```javascript
  let superman = await this.avengersUserService.addAvenger("Superman", "Clark", "Kent", [
    {name: "Flight"}, 
    {name: "Superhuman Strength"}, 
    {name: "Superhuman Speed"}, 
    {name: "Superhuman Breath"}, 
    {name: "X-Ray Vision"}, 
    {name: "Superhuman hearing"}, 
    {name: "Superhuman Vision"}])
  let batman = await this.avengersUserService.addAvenger("Batman", "Bruce", "Wayne", [])
  let room = await this.avengersChatService.newChat([spiderman.id, batman.id]);
  this.avengersChatService.tease(superman, batman, "You have no chance", room.id)
  this.avengersChatService.tease(batman, superman, "I have no power but I'm not afraid", room.id)
  ```
- Je ne peux pas utiliser mon _custom cloud service_ `AvengersSkillService` depuis mon front
