## Itération et propriétés des tableaux (Array)

Même si les tableaux javascripts sont en vérité des objets, il n'y a pas de raison d'utiliser
la boucle[`for in`](#object.forinloop) pour itérer à l'intérieur. En fait,
il y a même de nombreuses bonnes raisons pour **ne pas** utiliser 'for in' sur les tableaux.

> **Note:** Les tableaux JavaScript ne sont **pas** des *tableaux associatifs*. JavaScript  
> a seulement des [objects](#object.general) qui relient les paires clé-valeurs. Et tandis que les
> tableaux associatifs **preservent** l'ordre des éléments, les objets **ne le font pas**.

Puisque la boucle `for in` liste toutes les propriétés de la chaîne du prototype, 
et que le seul moyen d'exclure ces propriétés est d'utiliser
[`hasOwnProperty`](#object.hasownproperty), on a déjà un traitement jusqu'à **20 fois** 
plus lent qu'une classique boucle `for`.

### Itération

Pour obtenir les meilleures performances sur les itérations de tableaux, le mieux est donc d'utiliser
la boucle `for` classique.

    var list = [1, 2, 3, 4, 5, ...... 100000000];
    for(var i = 0, l = list.length; i < l; i++) {
        console.log(list[i]);
    }

L'astuce complémentaire sur l'exemple ci-dessus, c'est de mettre en cache la longueur
du tableau via l'utilisation d'une variable `l = list.length`.

Même si la propriété `length` property est définie dans le tableau lui-même, il y a encore
une surchauffe possible au fait d'analyser la totalité du tableau à chaque itération de la boucle.
Malgré les moteurs javascript récents qui **devraient** optimiser ce processus, il n'y aucun
moyen de savoir si le code va s'exécuter sur un moteur récent ou plus ancien.
En fait, ôter la gestion du cache pourrait provoquer une boucle seulement pour 
**moitié aussi rapide** qu'avec la longueur mise en cache.

### La propriété `length`

L'**appel** (getter) à la propriété `length` retourne le nombre d'éléments contenus dans le tableau,
alors que l'**affectation** (setter) peut être utilisé pour **tronquer** le tableau.

    var foo = [1, 2, 3, 4, 5, 6];
    foo.length = 3;
    foo; // [1, 2, 3]

    foo.length = 6;
    foo; // [1, 2, 3]

Assigner une longueur plus courte va tronquer le tableau, mais en revanche, affecter une valeur plus élevée n'a
aucun effet sur le tableau.

### En Conclusion

Pour les meilleures performances, il est recommandé d'utiliser la boucle `for`
et de mettre en cache la propriété `length`. L'utilisation de `for in` sur un tableau est un signe de code
mal écrit qui vous garantie à coup sûr erreurs et performances dégradées.
