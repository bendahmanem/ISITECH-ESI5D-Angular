Pour l'installer :

```sh
npm i -g @angular/cli@16.1.0
 ou
npm i -g @angular/cli
```

Ensuite pour verifier l'installation:

```sh
ng version
```

Pour créer un projet :

```sh
ng new ponyracer --defaults --prefix pr --standalone
```

Depuis la CLI v12, il n’y a plus de tests de bout en bout (end-to-end ou e2e) par défaut. Nous devons choisir une bibliothèque de test e2e, et l’ajouter au projet.

Nous allons utiliser Cypress. On pourrait le configurer à la main, mais le plus simple est d’utiliser une "schematic". La CLI est un outil très puissant. Elle permet aux développeurs de bâtir leurs propres "extensions" de la CLI (appelées "builders"), et leurs propres générateurs de code (appelés "schematics").

Cette schematic Cypress est la schematic officielle fournie par Cypress que nous allons utiliser pour ajouter Cypress à notre projet :

```sh
cd ponyracer
ng add @cypress/schematic@2.5.0 --e2e --no-component --skip-confirmation
```

Si vous avez une erreur avec la mention : 'peer-dependencies', executez la commande suivante :

```sh
npm config set legacy-peer-deps=true --location=project
```

Puis essayez a nouveau la commande ng add precedente

Pendant longtemps, la CLI incluait également l’outil TSLint par défaut pour vérifier notre code. Mais TSLint est maintenant déprécié, il n’est donc plus inclus depuis la CLI v12. Pas d’inquiétude cependant : nous allons utiliser ESLint à la place.

```sh
ng add @angular-eslint/schematics@16.0.3 --skip-confirmation
```

Pour demarrer l'application :

```ts
ng serve
```

Un composant est la combinaison d’une vue (le template) et de logique (notre classe TS). La CLI a d’ores et déjà créé un composant pour nous src/app/app.component.ts.

Par souci de simplicite je vais utiliser bootstrap pour le projet du cours :

```sh
npm install --save-exact bootstrap@5.3.0 font-awesome
```

et ajouter ces lignes au fichier styles.css :

```sh
@import 'bootstrap/dist/css/bootstrap.css';
@import 'font-awesome/css/font-awesome.css';
```

## Les composants

Notre application elle-même est un simple composant. Pour l’indiquer à Angular, on utilise le décorateur @Component. Et pour pouvoir l’utiliser, il nous faut l’importer comme tu peux le voir en haut du fichier.

Le décorateur @Component attend un objet de configuration.

Donc ici, chaque fois que notre HTML contiendra un élément <ns-root></ns-root>, Angular créera une nouvelle instance de notre classe AppComponent.\

Le template est le code HTML qui sera généré par Angular. Il est défini dans le fichier app.component.html ou il peut etre défini directement dans le décorateur @Component.

La propriété standalone: true est celle qui rend notre composant standalone. Cela signifie que nous n’aurons pas besoin de le déclarer dans un module Angular pour pouvoir l’utiliser.

L’autre propriété, imports: [CommonModule] n’est pas toujours nécessaire. Son rôle est de donner à Angular la liste des autres composants, directives et pipes qui peuvent être utilisés à l’intérieur du template de notre composant. La plupart des composants que nous créons utilisent des pipes et des directives fournies par Angular. Ces pipes et directives communément utilisés sont déclarés dans le module Angular CommonModule. Importer CommonModule dans la configuration rend ainsi possible l’utilisation des pipes et directives de CommonModule dans le template de notre composant.

## L'interpolation

L’interpolation est une fonctionnalité d’Angular qui permet d’afficher des valeurs dans le template. Pour l’utiliser, il suffit de placer une expression entre deux accolades {{ }}. L’expression peut être une variable, une propriété d’un objet, ou même une expression plus complexe.

```html
<h1>{{ title }}</h1>
```

Petite parenthese sur les optionnels:

    ```html
    {{user?.name}}
    ```

On appelle le `?` "optional chaining operator". Il permet de ne pas avoir d'erreur si l'objet est null ou undefined.

## Le binding de propriété

L’interpolation n’est qu’une des façons d’avoir des morceaux dynamiques dans nos templates.

En fait, l’interpolation n’est qu’une simplification du concept au cœur du moteur de template d’Angular : le binding de propriété.

En Angular, on peut écrire dans toutes les propriétés du DOM via des attributs spéciaux sur les éléments HTML, entourés de crochets []. Ça fait bizarre au premier abord, mais en fait c’est du HTML valide (et ça m’a aussi surpris). Un nom d’attribut HTML peut commencer par n’importe quoi, à l’exception de quelques caractères comme un guillemet ", une apostrophe ', un slash /, un égal =, un espace...

Je mentionne les propriétés du DOM, mais peut-être n’est-ce pas clair pour toi. On écrit en général dans des attributs HTML, n’est-ce pas ? Tout à fait, en général c’est ce qu’on fait. Prenons ce simple morceau d’HTML :

```html
<input type="text" value="Hello" />
```

Nous avons utilise l'interpolation pour afficher le contenu d'une variable:

```
<p>{{ user.name }}</p>
```

en realite cela correspond a ceci :

```html
<p [textContent]="user.name"></p>
```

```html
<option selected>Rainbow Dash</option>
```

```html
<option [selected]="isPonySelected" value="Rainbow Dash">Rainbow Dash</option>

<ns-pony name="Pony {{ pony.name }}"></ns-pony>

<ns-pony [name]="'Pony ' + pony.name"></ns-pony>
```

## Les events

Avec Angular nous disposons de **directives** specifiques a divers elements du DOM

````html
<button (click)="onButtonClick()">Click me</button>

``` html Au sein de votre composant races : ``` typescript @Component({
selector: 'ns-races', template: `
<h2>Races</h2>
<button (click)="refreshRaces()">Refresh the races list</button>
<p>{{ races.length }} races</p>
`, standalone: true }) export class RacesComponent { races: Array<RaceModel>
  = []; refreshRaces(): void { this.races = [{ name: 'London' }, { name: 'Lyon'
  }]; } }</RaceModel
>
````

Vous pouvez acceder a l'evenement lors de l'appel de la fonction :

```html
<div (click)="onButtonClick($event)"><button>Click me!</button></div>
```

```typescript
onButtonClick(event: MouseEvent): void {
  console.log(event);
  event.stopPropagation();
  event.preventDefault();
}
```

On peut aussi gerer les evenements du clavier:

```html
<textarea (keydown)="onKeyDown($event)"></textarea>
```

Je vais tenter de vous expliquer la difference entre :

```html
<component [property]="doSomething()"></component>
```

et

```html
<component (event)="doSomething()"></component>
```

Dans le premier cas de binding de propriété, la valeur doSomething() est une expression, et sera évaluée à chaque cycle de détection de changement pour déterminer si la propriété doit être modifiée.
Dans le second cas de binding d’événement, la valeur doSomething() est une instruction (statement), et ne sera évaluée que lorsque l’événement est déclenché.
Par définition, ces deux bindings ont des objectifs complètement différents, et comme tu t’en doutes,
des restrictions d’usage différentes. Par exemple, il est impossible d’utiliser un binding d’événement sur une propriété, et inversement.

```html
<component [property]="user = 'Cedric'"></component>
```

### Les variables locales

```html
<input #nameInput type="text" /> {{nameInput.value}}
```

On peut utiliser une methode standard de l'API du DOM avec une variable locale en Angular:

```html
<input type="text" #name />
<button (click)="name.focus()">Focus the input</button>
```

On peut aussi utiliser les variables locales avec des composants specifiques importe dans notre projet:

```html
<google-youtube #player></google-youtube>
<button (click)="player.play()">Play!</button>
```

## Les directives de structure

Les directives de structure sont des directives qui modifient la structure du DOM. Elles sont faciles à reconnaître : elles commencent par un astérisque \*.

```html
<div *ngIf="races.length > 0"><h2>Races</h2></div>
```

 <div *ngIf="races.length > 0">
        <h2>Races</h2>
        <ul>
          <li *ngFor="let race of races">{{ race.name }}</li>
        </ul>
</div>

```html
<div [ngSwitch]="messageCount">
  <p *ngSwitchCase="0">You have no message</p>
  <p *ngSwitchCase="1">You have a message</p>
  <p *ngSwitchDefault>You have some messages</p>
</div>
```

## Les directives de templating

### NgStyle

```html
<div [ngStyle]="{fontWeight: fontWeight, color: color}">I've got style</div>
```

### NgClass

```html
<div [class.awesome-div]="isAnAwesomeDiv()">I've got style</div>
```

```html
<div
  [class.awesome-div]="isAnAwesomeDiv()"
  [class.wonderful-div]="isAWonderfulDiv()"
>
  I've got style
</div>
```

```html
<div
  [ngClass]="{'awesome-div': isAnAwesomeDiv(), 'wonderful-div':isAWonderfulDiv()}"
>
  I've got style
</div>
```

## Resume
Le système de template d’Angular nous propose une syntaxe puissante pour exprimer les parties dynamiques de notre HTML. Elle nous permet d’exprimer du binding de données, de propriétés, d’événements, et des considérations de templating, d’une façon claire, avec des symboles propres :

- {{ }} pour l’interpolation
- [] pour le binding de propriété
- () pour le binding d’événement
- # pour les variables locales
- \* pour les directives de structure
