# Decouverte de TS

TypeScript, qui existe depuis 2012, est un sur-ensemble de JavaScript, ajoutant quelques trucs à ES5. Le plus important étant le système de type, lui donnant même son nom. Depuis la version 1.5, sortie en 2015, cette bibliothèque essaie d’être un sur-ensemble d’ES2015+, incluant toutes les fonctionnalités vues précédemment, et quelques nouveautés, comme les décorateurs. Ecrire du TypeScript ressemble à écrire du JavaScript. Par convention les fichiers sources TypeScript ont l’extension .ts, et seront compilés en JavaScript standard, en général lors du build, avec le compilateur TypeScript. Le code généré reste très lisible.

## Installation

```sh
npm i -g typescript
touch test.ts
tsc test.ts
```

Pour les erreurs d'execution de script ps1 sur windows :

```powershell
Set-ExecutionPolicy 'unrestricted'
```

## Types

La syntaxe pour ajouter des informations de type en TypeScript est basique :

```ts
let variable: type;

let a: number = 1;
```

Les types de bases sont :

number, string, boolean, any, void, null, undefined, never, enum, array, tuple, object, symbol, bigint

```ts
const ponyName: string = "Rainbow Dash";

const ponyAge: number = 42;

const isPonyAwesome: boolean = true;

const pony: any = "Rainbow Dash";

const pony: void = undefined;
```

TypeScript supporte aussi ce que certains langages appellent des types génériques, par exemple avec un Array :

```ts
const ponies: Array<string> = ["Rainbow Dash", "Pinkie Pie"];
```

Et comment faire si tu as besoin d’une variable pouvant recevoir plusieurs types ? TS a un type spécial pour cela, nommé **any**.

```ts
let changingType: any = "Rainbow Dash";
changingType = 42;
changingType = true;
```

C’est pratique si tu ne connais pas le type d’une valeur, soit parce qu’elle vient d’un bout de code dynamique, ou en sortie d’une bibliothèque obscure.

Si ta variable ne doit recevoir que des valeurs de type number ou boolean, tu peux utiliser l’union de types :

```ts
let changingType: number | boolean = 42;

changingType = true; // pas d'erreur de compilation
```

## Les enums

TypeScript propose aussi des valeurs énumérées : enum. Par exemple, une course de poneys dans ton
application peut être soit ready, started ou done.

```ts
enum RaceStatus {
  Ready,
  Started,
  Done,
}
```

```ts
const race = new Race();
race.status = RaceStatus.Ready;

enum Position {
  First = "First",
  Second = "Second",
  Other = "Other",
}
```

En general on prefere la syntaxe suivante qui utilise les unions de types :

```ts
let color: "blue" | "red" | "green";
```

## Les fonctions

### Le typage des paramètres et les types de retour

````ts

``` js
function startRace(race) {
    race.status = RaceStatus.Started;
    return race
}
````

```ts
function startRace(race: Race): Race {
  race.status = RaceStatus.Started;
  return race;
}

// On peut utiliser void si notre fonction ne retourne rien

function startRace(race: Race): void {
  race.status = RaceStatus.Started;
}
```

## Les interfaces

En JS, Une fonction marchera si elle reçoit un objet possédant la bonne propriété :

```js
function addPointsToScore(player, points) {
  player.score += points;
}
```

```ts
function addPointsToScore(player: { score: number }, points: number): void {
  player.score += points;
}
```

L'ideal dans ce cas de figure est d'utiliser une interface :

```ts
interface Player {
  score: number;
}

function addPointsToScore(player: Player, points: number): void {
  player.score += points;
}
```

Pour nos modeles on utilise des interfaces :

```ts
interface PonyModel {
  name: string;
  speed: number;
}
const pony: PonyModel = { name: "Light Shoe", speed: 56 };
```

### Paramètres optionnels

```ts
function addPointsToScore(player: HasScore, points?: number): void {
  points = points || 0;
  player.score += points;
}
```

### Des fonctions en propriété

```ts
interface CanRun {
  run(meters: number): void;
}
```

```ts
function startRunning(pony: CanRun): void {
  pony.run(10);
}

const ponyOne = {
  run: (meters: number) => console.log(`I'm running ${meters} meters`),
};

startRunning(ponyOne);
```
