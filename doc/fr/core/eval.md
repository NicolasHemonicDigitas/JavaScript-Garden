## Pourquoi `eval` ne doit pas �tre utilis�e

La fonction `eval` e�xute une cha�ne de code javascript dans le contexte loca.

    var foo = 1;
    function test() {
        var foo = 2;
        eval('foo = 3');
        return foo;
    }
    test(); // 3
    foo; // 1

Mais `eval` ne s'ex�cute dans le contexte local que quand elle est appel�e **directement**
**et** que le nom de la fonction appel�e est `eval`.

    var foo = 1;
    function test() {
        var foo = 2;
        var bar = eval;
        bar('foo = 3');
        return foo;
    }
    test(); // 2
    foo; // 3

L'utilisation de `eval` doit �tre �vit�e � **tout prix**. Dans 99.9% des cas o� on son usage "para�t utile",
le but peut �tre en r�alit� atteint **sans l'utiliser**.
    
### `eval` d�guis�

Les fonctions [timeout functions](#other.timeouts) `setTimeout` et `setInterval` peuvent
toutes les deux prendre une cha�ne de caract�res comme premier argument.
Cette cha�ne va **toujours** �tre ex�cut�e dans le scope global car `eval` n'a pas �t� 
appel�e directement dans ce cas.

### Probl�mes de s�curit�

`eval` pose �galement un probl�me de s�curit�, car il ex�cute **tout** code qui lui est envoy�;
ainsi, il ne doit **jamais** �tre utilis� avec des cha�nes de source inconnue ou non s�re.

### En Conclusion

`eval` ne doit jamais �tre utilis�e, tout code l'utilisant devant �tre remis en question dans son fonctionnement
ses performances, ou la s�curit�. Dans les rares cas ou quelque chose requiert `eval` pour fonctionner, 
son design doit �galement �tre remis en questions et ne devrait pas �tre utilis� � premi�re vue; 
un *meilleur design* devrait �tre utilis�, qui ne requiert pas l'usage de `eval`. 

