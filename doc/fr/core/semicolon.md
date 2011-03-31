## Insertion de point virgule automatique

Bien que JavaScript ait une syntaxe de style C, il ne force **pas**
à l'utilisation de points virgules dans le code source, 
il est donc possible de les omettre.

Toutefois, JavaScript n'est pas pour autant un langage sans point virgule, il a en réalité besoin de ces 
informations pour comprendre le code source. En l'occurrence, le parser JavaScript
va **automatiquement** les insérer à chaque fois qu'il va rencontrer une erreur de génération
liée à un point virgule manquant.

    var foo = function() {
    } // erreur d'analyse, point virgule attendu
    test()

L'insertion s'effectue, et le parser essaie à nouveau

    var foo = function() {
    }; // pas d'erreur, l'analyse continue
    test()

L'insertion automatique de ces points virgules est considérée comme l'une des **plus grosses**
faille de design du langage, puisqu'elle *peut* changer le comportement initialement prévu pour le code.

### Comme cela fonctionne

Le code ci-après n'a aucun point virgule, ce qui laisse donc tout le loisir au parser de décider 
où les insérer.

    (function(window, undefined) {
        function test(options) {
            log('testing!')

            (options.list || []).forEach(function(i) {

            })

            options.value.test(
                'longue chaine à passer ici',
                'et encore une autre longue chaine ici'
            )

            return
            {
                foo: function() {}
            }
        }
        window.test = test

    })(window)

    (function(window) {
        window.someLibrary = {}

    })(window)

Ci-dessous le résultat de ce "jeu de hasard" du parser :

    (function(window, undefined) {
        function test(options) {

            // Not inserted, lines got merged
            log('testing!')(options.list || []).forEach(function(i) {

            }); // <- inséré

            options.value.test(
                'longue chaine à passer ici',
                'et encore une autre longue chaine ici'
            ); // <- inséré

            return; // <- inséré, casse l'objet retourné
            { // donnée traitée comme un bloc

                // un libélé et une simple expression
                foo: function() {} 
            }; // <- inséré
        }
        window.test = test; // <- inséré

    // Les lignes sont fusionnée également
    })(window)(function(window) {
        window.someLibrary = {}; // <- inséré

    })(window); //<- inséré

> **Note:** Le parser JavaScript ne traite pas "correctement" les 'return' qui sont suivi
> d'une nouvelle ligne; même si ça n'est pas nécessairement la faute de l'insertion automatique
> des points virgules, cela peut tout de même être un effet de bord indésirable.

Le parser a ici complètement changé le comportement du code ci dessus, ce qui prouve que
dans certains cas, il fait les **mauvais choix**.

### Parenthèses prioritaires

En cas de parenthèses, le parser n'insère **pas** de point virgule.

    log('testing!')
    (options.list || []).forEach(function(i) {})

This code gets transformed into one line.

    log('testing!')(options.list || []).forEach(function(i) {})

Chances are **very** high that `log` does **not** return a function; therefore,
the above will yield a `TypeError` stating that `undefined is not a function`.

### In Conclusion

It is highly recommended to **never** omit semicolons, it is also advocated to 
keep braces on the same line with their corresponding statements and to never omit 
them for one single-line `if` / `else` statements. Both of these measures will 
not only improve the consistency of the code, they will also prevent the 
JavaScript parser from changing its behavior.

