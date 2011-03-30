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

Although the `length` property is defined on the array itself, there is still an
overhead for doing the lookup on each iteration of the loop. And while recent 
JavaScript engines **may** apply optimization in this case, there is no way of
telling whether the code will run on one of these newer engines or not. 

In fact, leaving out the caching may result in the loop being only **half as
fast** as with the cached length.

### The `length` Property

While the *getter* of the `length` property simply returns the number of
elements that are contained in the array, the *setter* can be used to 
**truncate** the array.

    var foo = [1, 2, 3, 4, 5, 6];
    foo.length = 3;
    foo; // [1, 2, 3]

    foo.length = 6;
    foo; // [1, 2, 3]

Assigning a smaller length does truncate the array, but increasing the length 
does not have any effect on the array.

### In Conclusion

For the best performance it is recommended to always use the plain `for` loop
and cache the `length` property. The use of `for in` on an array is a sign of
badly written code that is prone to bugs and bad performance. 

