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
