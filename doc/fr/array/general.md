## It�ration et propri�t�s des tableaux (Array)

M�me si les tableaux javascripts sont en v�rit� des objets, il n'y a pas de raison d'utiliser
la boucle[`for in`](#object.forinloop) pour it�rer � l'int�rieur. En fait,
il y a m�me de nombreuses bonnes raisons pour **ne pas** utiliser 'for in' sur les tableaux.

> **Note:** Les tableaux JavaScript ne sont **pas** des *tableaux associatifs*. JavaScript  
> a seulement des [objects](#object.general) qui relient les paires cl�-valeurs. Et tandis que les
> tableaux associatifs **preservent** l'ordre des �l�ments, les objets **ne le font pas**.

Puisque la boucle `for in` liste toutes les propri�t�s de la cha�ne du prototype, 
et que le seul moyen d'exclure ces propri�t�s est d'utiliser
[`hasOwnProperty`](#object.hasownproperty), on a d�j� un traitement jusqu'� **20 fois** 
plus lent qu'une classique boucle `for`.

### It�ration

Pour obtenir les meilleures performances sur les it�rations de tableaux, le mieux est donc d'utiliser
la boucle `for` classique.

    var list = [1, 2, 3, 4, 5, ...... 100000000];
    for(var i = 0, l = list.length; i < l; i++) {
        console.log(list[i]);
    }

L'astuce compl�mentaire sur l'exemple ci-dessus, c'est de mettre en cache la longueur
du tableau via l'utilisation d'une variable `l = list.length`.

M�me si la propri�t� `length` property est d�finie dans le tableau lui-m�me, il y a encore
une surchauffe possible au fait d'analyser la totalit� du tableau � chaque it�ration de la boucle.
Malgr� les moteurs javascript r�cents qui **devraient** optimiser ce processus, il n'y aucun
moyen de savoir si le code va s'ex�cuter sur un moteur r�cent ou plus ancien.
En fait, �ter la gestion du cache pourrait provoquer une boucle seulement pour 
**moiti� aussi rapide** qu'avec la longueur mise en cache.

### La propri�t� `length`

L'**appel** (getter) � la propri�t� `length` retourne le nombre d'�l�ments contenus dans le tableau,
alors que l'**affectation** (setter) peut �tre utilis� pour **tronquer** le tableau.

    var foo = [1, 2, 3, 4, 5, 6];
    foo.length = 3;
    foo; // [1, 2, 3]

    foo.length = 6;
    foo; // [1, 2, 3]

Assigner une longueur plus courte va tronquer le tableau, mais en revanche, affecter une valeur plus �lev�e n'a
aucun effet sur le tableau.

### En Conclusion

Pour les meilleures performances, il est recommand� d'utiliser la boucle `for`
et de mettre en cache la propri�t� `length`. L'utilisation de `for in` sur un tableau est un signe de code
mal �crit qui vous garantie � coup s�r erreurs et performances d�grad�es.
