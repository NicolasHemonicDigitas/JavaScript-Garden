## Le contructeur `Array` 

Malheureusement le constructeur `Array` est assez ambig� sur sa gestion des param�tres;
*il est donc hautement recommand� de toujours utiliser la syntaxe lit�rale, avec les crochets - `[]` - 
quand on cr�� un tableau.

    [1, 2, 3]; // Resultat: [1, 2, 3]
    new Array(1, 2, 3); // Resultat: [1, 2, 3]

    [3]; // Resultat: [3]
    new Array(3); // Resultat: []
    new Array('3') // Resultat: ['3']

Dans les cas o� un seul argument est pass� au constructeur `Array`,
et que cet argument est un nombre (`Number`),le constructeur va retourner un nouveau tableau *interm�diare*
avec la propri�t� `length` renseign�e � la valeur de l'argument. Il est � noter que **seule** la propri�t�
`length` de ce nouveau tableau sera renseign�e par ce biais, les index du tableau ne seront pas r�initialis�s.

    var arr = new Array(3);
    arr[1]; // undefined
    1 in arr; // false, l'index n'a pas �t� affect�

Le comportement consistant � affecter la longueur du tableau en amont
ne s'av�re vraiment utile que dans des cas bien pr�cis, comme
r�peter une chaine,ce qui �viter d'utiliser une boucle `for`.

    new Array(count + 1).join(stringToRepeat);

### En Conclusion

L'utilisation du constructeur `Array` doit �tre �vit�e le plus possible.
L'�criture lit�rale avec les crochets est pr�f�rable. Elle s'av�re plus courte et sa syntaxe plus compr�hensible,
ce qui accroit la lisibilit� du code.

