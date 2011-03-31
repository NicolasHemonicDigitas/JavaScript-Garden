## Insertion de point virgule automatique

Bien que JavaScript ait une syntaxe de style C, il ne force **pas**
� l'utilisation de points virgules dans le code source, 
il est donc possible de les omettre.

Toutefois, JavaScript n'est pas pour autant un langage sans point virgule, il a en r�alit� besoin de ces 
informations pour comprendre le code source. En l'occurrence, le parser JavaScript
va **automatiquement** les ins�rer � chaque fois qu'il va rencontrer une erreur de g�n�ration
li�e � un point virgule manquant.

    var foo = function() {
    } // erreur d'analyse, point virgule attendu
    test()

L'insertion s'effectue, et le parser essaie � nouveau

    var foo = function() {
    }; // pas d'erreur, l'analyse continue
    test()

L'insertion automatique de ces points virgules est consid�r�e comme l'une des **plus grosses**
faille de design du langage, puisqu'elle *peut* changer le comportement initialement pr�vu pour le code.

### Comme cela fonctionne

Le code ci-apr�s n'a aucun point virgule, ce qui laisse donc tout le loisir au parser de d�cider 
o� les ins�rer.

    (function(window, undefined) {
        function test(options) {
            log('testing!')

            (options.list || []).forEach(function(i) {

            })

            options.value.test(
                'longue chaine � passer ici',
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

Ci-dessous le r�sultat de ce "jeu de hasard" du parser :

    (function(window, undefined) {
        function test(options) {

            // Not inserted, lines got merged
            log('testing!')(options.list || []).forEach(function(i) {

            }); // <- ins�r�

            options.value.test(
                'longue chaine � passer ici',
                'et encore une autre longue chaine ici'
            ); // <- ins�r�

            return; // <- ins�r�, casse l'objet retourn�
            { // donn�e trait�e comme un bloc

                // un lib�l� et une simple expression
                foo: function() {} 
            }; // <- ins�r�
        }
        window.test = test; // <- ins�r�

    // Les lignes sont fusionn�e �galement
    })(window)(function(window) {
        window.someLibrary = {}; // <- ins�r�

    })(window); //<- ins�r�

> **Note:** Le parser JavaScript ne traite pas "correctement" les 'return' qui sont suivi
> d'une nouvelle ligne; m�me si �a n'est pas n�cessairement la faute de l'insertion automatique
> des points virgules, cela peut tout de m�me �tre un effet de bord ind�sirable.

Le parser a ici compl�tement chang� le comportement du code ci dessus, ce qui prouve que
dans certains cas, il fait les **mauvais choix**.

### Parenth�ses prioritaires

En cas de parenth�ses, le parser n'ins�re **pas** de point virgule.

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

