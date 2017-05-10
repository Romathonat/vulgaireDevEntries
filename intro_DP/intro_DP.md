## Intro
Dynamic Programming. Ou programmation dynamique en français. C'est une technique de résolution de problèmes, qui n'est pas si évidente 
à comprendre, du moins quand on débute. 

Le principe de la programmation dynamique est d'utiliser la récursivité d'un problème, on parle de **sous-structure optimale**.
Cela consiste à prendre un problème de taille N, le résoudre pour les versions de taille moindre, de la plus petite à la plus plus grande,
en stockant les résultats intermédiaires (un genre de [mémoïsation](https://fr.wikipedia.org/wiki/M%C3%A9mo%C3%AFsation)).

Alors bien sûr, on ne peut pas utiliser la programmation dynamique pour résoudre tous les problèmes, ce serait trop simple. Il faut
que celui-ci présente une sous-structure optimale.

## Le problème du sac à dos
Prenons un bon exemple pour mieux comprendre. Vous possédez un sac à dos qui peut contenir un poids maximal W. Vous possédez i objets, que vous pouvez mettre dans le sac à dos, chacun ayant un poids wi et une valeur vi. Le but est de trouver l'ensemble des objets i qui maximise la somme des valeurs que vous avez dans votre sac, sans dépasser le poids W.  

On pourrait se dire "c'est facile, il suffit de commencer par mettre les objets les plus lourds, puis les plus légers et ça marche".  

*Contre exemple*: W = 5, objets = {(5, 2), (2, 3), (2, 3)} (le premier terme est le poids, le second est la valeur.
En applicant cela, on se retrouve avec un seul objet de poids 5, pour une valeur de 2, alors que si on avait mis les deux objets de poids 2, on aurait une valeur totale de 4, ce qui aurait été mieux.

On pourrait aussi se dire "bon ben alors, il faut commencer par mettre les objets de plus fortes valeurs en premier".
    
*Contre-exemple*: W = 5, objets = {(5,4), (2,2), (1,3)}

Pareil, on se retrouverait avec une valeur totale de 4 avec un objet de poids 5, alors que mettre les deux objets (2,2) et (1,3) auraient constitué une valeur totale de 5.

En fait ces deux algorithme sont des algorithmes **gloutons**

