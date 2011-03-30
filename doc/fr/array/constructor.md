## Le contructeur `Array` 

Malheureusement le constructeur `Array` est assez ambigü sur sa gestion des paramètres;
*il est donc hautement recommandé de toujours utiliser la syntaxe litérale, avec les crochets - `[]` - 
quand on créé un tableau.

    [1, 2, 3]; // Resultat: [1, 2, 3]
    new Array(1, 2, 3); // Resultat: [1, 2, 3]

    [3]; // Resultat: [3]
    new Array(3); // Resultat: []
    new Array('3') // Resultat: ['3']

Dans les cas où un seul argument est passé au constructeur `Array`,
et que cet argument est un nombre (`Number`),le constructeur va retourner un nouveau tableau *intermédiare*
avec la propriété `length` renseignée à la valeur de l'argument. Il est à noter que **seule** la propriété
`length` de ce nouveau tableau sera renseignée par ce biais, les index du tableau ne seront pas réinitialisés.

    var arr = new Array(3);
    arr[1]; // undefined
    1 in arr; // false, l'index n'a pas été affecté

Le comportement consistant à affecter la longueur du tableau en amont
ne s'avère vraiment utile que dans des cas bien précis, comme
répeter une chaine,ce qui éviter d'utiliser une boucle `for`.

    new Array(count + 1).join(stringToRepeat);

### En Conclusion

L'utilisation du constructeur `Array` doit être évitée le plus possible.
L'écriture litérale avec les crochets est préférable. Elle s'avère plus courte et sa syntaxe plus compréhensible,
ce qui accroit la lisibilité du code.

