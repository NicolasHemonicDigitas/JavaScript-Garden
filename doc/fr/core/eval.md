## Pourquoi `eval` ne doit pas être utilisée

La fonction `eval` eéxute une chaîne de code javascript dans le contexte loca.

    var foo = 1;
    function test() {
        var foo = 2;
        eval('foo = 3');
        return foo;
    }
    test(); // 3
    foo; // 1

Mais `eval` ne s'exécute dans le contexte local que quand elle est appelée **directement**
**et** que le nom de la fonction appelée est `eval`.

    var foo = 1;
    function test() {
        var foo = 2;
        var bar = eval;
        bar('foo = 3');
        return foo;
    }
    test(); // 2
    foo; // 3

L'utilisation de `eval` doit être évitée à **tout prix**. Dans 99.9% des cas où on son usage "paraît utile",
le but peut être en réalité atteint **sans l'utiliser**.
    
### `eval` déguisé

Les fonctions [timeout functions](#other.timeouts) `setTimeout` et `setInterval` peuvent
toutes les deux prendre une chaîne de caractères comme premier argument.
Cette chaîne va **toujours** être exécutée dans le scope global car `eval` n'a pas été 
appelée directement dans ce cas.

### Problèmes de sécurité

`eval` pose également un problème de sécurité, car il exécute **tout** code qui lui est envoyé;
ainsi, il ne doit **jamais** être utilisé avec des chaînes de source inconnue ou non sûre.

### En Conclusion

`eval` ne doit jamais être utilisée, tout code l'utilisant devant être remis en question dans son fonctionnement
ses performances, ou la sécurité. Dans les rares cas ou quelque chose requiert `eval` pour fonctionner, 
son design doit également être remis en questions et ne devrait pas être utilisé à première vue; 
un *meilleur design* devrait être utilisé, qui ne requiert pas l'usage de `eval`. 

