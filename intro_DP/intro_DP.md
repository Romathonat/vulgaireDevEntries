## Intro
Dynamic Programming. Ou programmation dynamique en français. C'est une technique de résolution de problèmes, qui n'est pas si évidente 
à comprendre, du moins quand on débute. 

Le principe de la programmation dynamique est d'utiliser la récursivité d'un problème, on parle de **sous-structure optimale**.
Cela consiste à prendre un problème de taille N, le résoudre pour les versions de taille moindre, de la plus petite à la plus plus grande,
en stockant les résultats intermédiaires (un genre de [mémoïsation](https://fr.wikipedia.org/wiki/M%C3%A9mo%C3%AFsation)).

Alors bien sûr, on ne peut pas utiliser la programmation dynamique pour résoudre tous les problèmes, ce serait trop simple. Il faut
que celui-ci présente une sous-structure optimale.
