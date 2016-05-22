# JavaScriptTutorialFR
JavaScriptTutorialFR ≥ console.log('coin coin');

###Un peu d’histoire.
JavaScript a été inventé par NetScape dans le but de créer un langage de script inspiré de Java mais rendu plus simple en syntaxe. Il a été placé dans le navigateur en 1995. Microsoft a tenté de concurencer ce langage de script avec JScript. Aujourd’hui encore JavaScript est une marque.

###Les normes.
En 1999 le normage de JavaScript a été demandé à la société Ecma. C’est pourquoi on utilise depuis les normes ECMAScript.

Ces normes ont une forte importance car elles permettent l’implémentation de nouvelles fonctionnalitées des langages modernes dans tous les moteurs JavaScript, qu’ils soient dans les navigateurs ou server-side.

Aujourd’hui, les navigateurs comme Chrome, Firefox et Edge étendent peu à peu leurs compatibilité entre les versions 5 et 6 d’ECMAScript qui, lui est arrivé à la version 7.

Beaucoup de bibliothèques logicielles modernes très utilisé ont étées reprises en natif dans JavaScript graçe aux normes ECMAScript.

Si vous souhaitez savoir si une fonctionnalitée est compatible avec un navigateur ou un autre, alors dirigez-vous sur cet excellent tableau comparatif : 
http://kangax.github.io/compat-table/es6/

###Le mode strict.
L’un des autres apports d’ECMAScript est le mode strict.
Pour l’utiliser, il suffit de saisir l’instruction suivante : 
`'use strict';`

Ce mode active la norme ECMAScript5 et permet entre autre d’activer des logs de warnings dans la console en cas de comportement dépréciés ou d’usage de variable non définie.
Utiliser cette instruction pourrait ou devrait être appellée au plus haut niveau d’une application.

###Les types d’élements.
JavaScript possède deux catégories de variables :

* Les types primitifs :
    * string
    * number
    * boolean.

Par définition, un type de variable primitif n’est pas un objet.

* Les objets, dont les tableaux qui sont des objets.

On dit d’un objet qu’il est «typé» si il est instancié par un constructeur.
Si l’objet est typé, alors il a une propriété nommée «prototype» qui est elle-même un objet.

On peut donner à un objet des propriétés ou des méthode si la propriété est une fonction.

###Le scope (portée des déclarations).
Dans JavaScript, on doit déclarer les variables avant de les utiliser (si l’on suis la norme ECMAScript).

Cela se fait grace de la sorte : 

```javascript
var myString  = 'test';
var myArray   = [];
var myInteger = 2501;
var myFloat   = '1.1';

console.log(myString, myArray, myInteger, myFloat);
// test [] 2501 1.1
```

La portée d’une variable, son scope est celle de l’objet dans laquelle elle se trouve et non du bloc/bracket dans laquelle elle se trouve.

Une variable est donc globale (en fait précisemment, elle est déclarée dans this.window, mais elle est accessible de partout) quand celle ci n’est déclaré dans aucune fonction.

```javascript
/**
 * Itérateur d’interaction avec les femmes.
 *
 * @yield {int} Niveau de déception.
 */
function *girlInteractions() {

    // Variable locale.
    var levelOfDisapointement;

    // Renvoie du niveau de déception pour chaque nouvelle interaction.
    for (levelOfDisapointement = 1; true; levelOfDisapointement++) {
        yield levelOfDisapointement * levelOfDisapointement;
    }
}

// Déclaration d’une rencontre entre un nerd et une femme.
var randomGirl = girlInteractions();

// Première interaction.
console.log(
    'Accumulation de la déception lors de la première interaction ', randomGirl.next().value
);

// Deuxième interaction.
console.log(
    'Accumulation de la déception lors de la deuxième interaction ', randomGirl.next().value
);

// Troisième interaction.
console.log(
    'Accumulation de la déception lors de la troisième interaction ', randomGirl.next().value
);

// Un debug qui ne vas pas fonctionner étant donné qu’on éssaie d’afficher une variable locale de la fonction girlInteractions.
console.log(levelOfDisapointement);

```

Deuxième exemple : On peut constater que la portée d’une variable est étendue dans toute sa fonction et non dans son bloc.

```javascript
(function(){
    // Boucle d’une seule itération (juste pour l’exemple).
    for (i = 0; i < 1; i++) {
        var mySecondeGlobale = 'Je constate que la portée de la variable est celle de la fonction et non du bloc.';
    }

    // On doit pouvoir afficher la variable.
    console.log(mySecondeGlobale);
})();

// Ici on ne doit pas pouvoir afficher la variable.
console.log(mySecondeGlobale);
```

Troisième exemple : On peut utiliser une variable avant de l’avoir déclarer.
Cet usage doit être banni car il entretien la confusion.

```javascript
// Utilisation d’une variable globale dans une fonction.
(function(){
    console.log(myGlobal);
})();

// Déclaration de la variable globale après l’avoir utilisé.
var myGlobal = 'test';

// On peut constater que la valeur a été déclarée.
(function(){
    console.log(myGlobal);
})();

```

###Window / this.
`Window` est par défaut l’objet courrant de la fenêtre. Cet objet contient de nombreux objets que l’on peut débugguer graçe à `console.log(window);`.

Il est accessible par window mais aussi par this.window quand on le scope courant n’est pas une fonction.

Le BOM (Browser object model) est décrit entièrement dans l’objet window.

###Les prototypes.
Chaque objet JavaScript possède un prototype qui lui même est un objet.
Chaque objet hérite des propriétées et méthodes de son prototype, et aussi du prototype de ses parents.

Un autre exemple, on peut aussi voir aussi le prototype d’un Array de la sorte :
`console.log(window.Array.prototype);`

Prenons l’exemple suivant dans une console JavaScript : 

```javascript
/**
 * Déclaration d’un prototype de voiture «Car»
 *
 * @param  {string} brand  Marque de la voiture.
 * @param  {string} model  Model de la voiture.
 * @param  {int}    wheels Nombre de roues si il n’y en pas 4.
 *
 * @return {object} Une voiture.
 */
var car = function car( brand, model, wheels ) {
    this.brandName = brand;
    this.modelName = model;

    // Gestion du nombre de roues.
    if(typeof wheels === "undefined") {
        this.wheelsNumber = 4;
    } else {
        this.wheelsNumber = wheels;
    }
}

// Création d’une voiture à l’aide de notre prototype.
var rx8 = new car('Mazda', 'Rx8 R3');
console.log(rx8);

// Ajout d’une propriété à notre object créé par prototype.
rx8.wheelBase = 2703;

// Ajout d’une méthode à notre prototype.
car.prototype.getWheelBase = function() {
    return this.wheelBase;
}

// Ajout d’une méthode à notre objet.
rx8.getEngineType = function() {
    return 'rotary';
};

// Obtention de l’empattement graçe à notre prototype.
console.log(rx8.getWheelBase());

// Obtention du type de moteur graçe à notre objet.
console.log(rx8.getEngineType());

```

On peut voir que j’ai réussi à ajouter une méthode au prototype alors même que l’objet a déjà été créé.

J’aurai pu aussi ajouter directement la méthode à l’objet final, ou ajouter une propriété à la place d’une méthode, tant à l’objet final qu’à son prototype.

Les prototypes peuvent nous aider à créer de mécanismes d’héritages.

###Les fonctions.

####IIFE
En JavaScript, une fonction anonyme ou non qui s’appelle elle-même immédiatement se nomme une IIFE pour «Immediately-Invoked Funtion Expression».

Les avantages :
* La fonction possède son propre scope. De ce fait, ses variables internes ne perturbent pas le reste du code et vice versa.
* Le code est découpé et donc plus facile à déplacer / tester.

Un exemple :

```javascript
(function (local_arg) {
   // anonymous function
   console.log(local_arg);
})('hello');
```

####Les callback.
Un callback est une fonction passée en paramètre d’une autre fonction.
**WIP**

####Les closures.
Une closure est une fonction qui fait appel à une ressource n’étant pas un paramètre de la fonction.

http://openclassrooms.com/courses/les-closures-en-javascript

Voici un exemple d’un code dont le métier est buggué :

```javascript
// IIFE qui est censé afficher les nombres de i à 10;
(function() {
    for (var i = 0; i < 10; i++) {
        setTimeout(function() {
            console.log('i=' + i);
        }, 1000);
    }
})();
```

Mais par quelle sorcellerie mon pointeur se perd et n’est pas incrémenté?

Voilà le comportement détaillé du code :

* On déclare une fonction.
* On lance une boucle de i à 10 tout en créant la variable i pour tout le scope de la fonction.
* On itère 10 fois et on créé des appels avec callback dans 1000 millisecondes.
* La fonction IIFE se termine.
* Les callback sont lancés au bout du temps éscompté et affiche i. Mais que vaut i? i a été incrémenté 10 fois dans la boucle, donc on affiche 10 fois «i = 10».

Résolvons ce problème avec une closure :

```javascript
// IIFE qui vas afficher les nombres de 0 à 9 avec une fonction closure;
(function() {
    for (var i = 0; i < 10; i++) {
        (function(savedIValue) {
            setTimeout(function() { console.log('i=' + savedIValue); }, 1000);
        })(i);
    }
})();
```

Voilà le comportement détaillé du code :

* On déclare une fonction.
* On lance une boucle de i à 10 tout en créant la variable i pour tout le scope de la fonction.
* On itère 10 fois et on appelle une fonction avec un paramètre valant i.
* Cette fonction a son propre scope. De ce fait, pour chaque paramètre reçu, elle a déclaré une variable locale «savedIdValue» qui vaut i à l’instant «T».
* La fonction IIFE se termine.

Classe non?

###Tips & bonnes pratiques.

####Le wrapper de fonctionnalitées parfait.

* 1) Pour ne pas hériter de toutes les variables déclarées en global ou que notre propre code soit perturbé par du code externe on commence par déclarer notre code dans une IIFE.
* 2) On commence la première ligne de notre script par un `;`. De la sorte, on évite qu’une instruction d’un script externe du dom ne puisse faire planter notre code.
* 3) On utilise la norme ECMAScript5 avec `use script`;

```javascript
;(function() {
    console.log('Hello World');
})();
```

####Le Floating-point Guide.

Si l’on éxecute le code suivant, quel en sera le résultat selon vous?

```javascript
var wtf = function() {
    console.log(4.6 + 5.3);
}();
```

Cela est du à la manière dont JavaScript intéroge le processeur pour faire ses calculs.

De ce fait, méfiance avec la moindre addition. Il faut soit utiliser des arrondis manuels, soit utiliser des typages de variables monétaires.
Des tips : http://stackoverflow.com/questions/1458633/how-to-deal-with-floating-point-number-precision-in-javascript
