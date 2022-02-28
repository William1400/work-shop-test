# INTRODUCTION
## PRÉSENTATION DE TYPESCRIPT
TypeScript est un langage  à typage statique conçu par Anders Heljsberg (également concepteur du langage C#). Son but est de rendre plus fiable et facile l'écriture du code en JavaScript. Le code en TS sera compilé en JS. (comme SCSS)

## Quels sont les avantages de TS?
- Limites les erreurs
- Meilleure autocomplétion et documentation
- Cible la version de compilation du code TS en JS (ES5,ES6, etc)

## Quels sont les désavantages de TS?
- Outil supplémentaire
- L'écosystème JavaScript
- Perte en flexibilité (mais gagne en prévisibilité)
- Code moins lisible
  
## A QUEL MOMENT UTILISER TYPESCRIPT ?
TS permet d'apporter une rigueur dans votre code et sa prévisibilité permet d'éviter les bugs. Son ciblage permet de nous passer d'outil comme "Babel" par exemple. C'est pourquoi nous vous conseillons d'utiliser TS pour de gros projets.

## TSCONFIG
La présence d'un fichier tsconfig.json dans un répertoire indique que ce dernier est la racine d'un projet TypeScript. Le fichier tsconfig.json spécifie les fichiers racine et les options de compilation nécessaires pour compiler le projet.

Exemple:


```tsx
{
  "compilerOptions": {
    "module": "commonjs",
    "noImplicitAny": true,
    "removeComments": true,
    "preserveConstEnums": true,
    "sourceMap": true
  },
  "files": [
    "core.ts",
    "sys.ts",
    "types.ts",
    "scanner.ts",
    "parser.ts",
    "utilities.ts",
    "binder.ts",
    "checker.ts",
    "emitter.ts",
    "program.ts",
    "commandLineParser.ts",
    "tsc.ts",
    "diagnosticInformationMap.generated.ts"
  ]
}
```
**compilerOptions**: Ces options constituent la majeure partie de la configuration et couvrent la manière dont le langage doit fonctionner.

**module**: Définit le système de modules pour le programme.

**noImplicitAny**: En activant noImplicitAny, TypeScript émettra toutefois une erreur chaque fois qu'il en aura déduit une.

**removeComments**: Supprime tous les commentaires des fichiers TypeScript lors de la conversion en JavaScript. La valeur par défaut est false.

**preserveConstEnums**: N'efface pas les déclarations const enum dans le code généré. Les const enum permettent de réduire l'empreinte mémoire globale de votre application au moment de l'exécution en émettant la valeur de l'enum au lieu d'une référence.

**sourceMap**: Permet la génération de fichiers sourcemap. Ces fichiers permettent aux débogueurs et autres outils d'afficher le code source TypeScript original lorsqu'ils travaillent avec les fichiers JavaScript émis.

  

## TYPAGE
Le principal apport du langage TypeScript est la possibilité d'associer un type à une donnée.

```
let result: number;
let message: string;
let response: boolean;
let obj: any;
```
- **number** : pour les nombres entiers ou flottants.
- **string** : pour les chaînes de caractères.
- **boolean** : pour les booléens donc true ou false.
- **any** : type par défaut attribuer par TS à une variable globale s'il ne reconnait pas son type lors de sa déclaration.

Le typage peut aussi se faire implicitement:
``let message = "Hello World"  // message sera un string``

## INTERFACE
L'interface est une autre façon de typer un objet

```
interface{
  lastname: string,
  firstname: string,
  age: number,

}
```

## TYPE VS INTERFACE ?
Quelle est la différence entre les types et les interfaces ?
Les types sont plus rigides que les interfaces car une fois déclarer aucune extension n'est posssible.
Exemple:

Ce code provoquera une erreur

```
type Person = {
  lastname: string,
  firstname: string,
}

type Person = {
  age: number
}

```
Si nous souhaitons rajouter plus loin la variable **age** dans notre objet nous ne pouvons pas y faire référence à un autre endroit dans le code. Or c'est possible avec les interfaces.

Dans cette situation, préférez donc ceci:

```
interface Person{
  lastname: string,
  firstname: string,
}

interface Person{
  age: number
}
```

Notre interface **Person** a déjà été déclarer, elle sera reconnu par TS et implémentera **age**.


## CLASSES

Enfin, étendons l'exemple une dernière fois avec les classes. TypeScript prend en charge de nouvelles fonctionnalités de JavaScript, comme la prise en charge de la programmation orientée objet basée sur des classes.

Ici, nous allons créer une classe Student avec un constructeur et quelques champs publics. Remarquez que les classes et les interfaces fonctionnent bien ensemble, laissant le programmeur décider du bon niveau d'abstraction.

Notez également que l'utilisation de public comme argument du constructeur est un raccourci qui nous permet de créer automatiquement des propriétés portant ce nom.

```
class Student {
  fullName: string;
  constructor(
    public firstName: string,
    public middleInitial: string,
    public lastName: string
  ) {
    this.fullName = firstName + " " + middleInitial + " " + lastName;
  }
}
 
let user = new Student("Jane", "M.", "User");
 

```



# HOW TO USE IT

Commençons par créer une première application Web simple avec TypeScript.


## Installation de TypeScript

Il y a deux façons principales d'obtenir le TypeScript disponible pour votre projet :

- Via npm (le gestionnaire de paquets Node.js)
- En installant les plugins Visual Studio de TypeScript

Visual Studio 2017 et Visual Studio 2015 Update 3 incluent la prise en charge du langage TypeScript par défaut mais n'incluent pas le compilateur TypeScript, tsc. Si vous n'avez pas installé TypeScript avec Visual Studio, vous pouvez toujours le télécharger.

Pour les utilisateurs de npm :

``npm install -g typescript``


## Création de votre premier fichier TypeScript

Dans votre éditeur, tapez le code JavaScript suivant dans greeter.ts :

```
function greeter(person) {
  console.log("Hello, " + person);
}
 
let user = "John Doe";
 
greeter(user);
```

## Compilation de votre code

Nous avons utilisé une extension .ts, mais ce code n'est que du JavaScript. Vous auriez pu le copier/coller directement à partir d'une application JavaScript existante.

En ligne de commande, lancez le compilateur TypeScript :

``tsc greeter.ts``

Le résultat sera un fichier greeter.js qui contient le même JavaScript que celui que vous avez entré. Nous sommes prêts à utiliser TypeScript dans notre application JavaScript !

Nous pouvons maintenant commencer à tirer parti de certains des nouveaux outils offerts par TypeScript. Ajoutez une annotation de type : string à l'argument de la fonction 'person' comme indiqué ici :

```
function greeter(person: string) {
  console.log("Hello, " + person);
}
 
let user = "John Doe";

greeter(user);
```

## Annotations de type

Les annotations de type dans TypeScript sont des moyens légers d'enregistrer le type prévu de la fonction ou de la variable. Dans ce cas, nous souhaitons que la fonction greeter soit appelée avec un seul paramètre de type string. Nous pouvons essayer de modifier l'appel de la fonction greeter pour passer un tableau à la place :




## Variable de type
Vous pouvez créer une variable de type*:
```
type Age = number
```
Il est aussi possible de combiner plusieurs types avec les opérateurs:
```
type Message = string | null
```

## Classe

## Connexion à l'api REST API 


