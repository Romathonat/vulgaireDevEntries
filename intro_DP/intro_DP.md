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

En fait ces deux algorithme sont des algorithmes **gloutons**, c'est à dire qu'à chaque étape on fait un choix qui sur le coup nous 
paraît le meilleur, mais sur le long terme il ne l'est pas forcément. Par exemple, si on considère que notre problème est le bien de l'humanté, utiliser des énergies fossiles peut paraître être un bon choix localement dans le temps, puisque ça permet l'air industriel, le développement économique et technique, par contre sur le long terme, ce n'est pas la solution optimale puisque ça nous mène à des problèmes climatiques (entre autres) qui sont néfastes à l'humanité :)

Si on regarde l'espace des solutions possibles, on se rend vite compte que pour n objets, il y a 2^n possibilités. En effet, on peut associer ces solutions à un mot de n bits, chaque bit représentant si on met l'objet dans le sac ou pas (si vous ne comprenez pas pourquoi, aller regarder [le premier article de ce blog](http://vulgairedev.fr/blog/article/2-puissance-n).

En fait, on est face à un problème NP-complet. On peut cependant se débrouiller avec un algorithme dit [pseudo-polynomial](https://en.wikipedia.org/wiki/Pseudo-polynomial_time), en 
programmation dynamique !

## Solution

Notre problème dépend de deux paramètres: les éléments que l'on met dans le sac, ainsi que la contenance du sac. Notons-le KP(i, w)(KP pour Knapsack Problem), avec i signifiant qu'on prend le i premiers objets. KP(i, w) nous donne la **somme maximale des valeurs maximale qu'on puisse avoir**.

On peut voir que dans ce problème du sac à dos, on a une sous-structure optimale.
En effet, on a:

si wi > w : KP(i, w) = KP(i-1, w)
sinon: KP(i, w) = max(KP(i-1, w), KP(i-1, w-wi) + vi) 

Bon, c'est peut être pas ce qu'il y a de plus compréhensible écrit comme ça. J'essaie de l'expliquer mais c'est assez difficile.
En fait, ça veut dire que quand on prend le problème au rang i, on a deux cas:
- Soit le iéme élément a un poids wi supérieur à w, du coup on ne peut pas le mettre dans le sac. Du coup pour avoir la solution au problème, il suffit de regarder quelle est la valeur de KP au rang i-1 avec la même contenance w.
- Soit le ième élément peut être mis dans le sac. Dans ce cas, il faut prendre la valeur maximale entre KP(i-1, w), qui est la valeur du problème au rang i-1 (sans prendre le iéme élément), et KP(i-1, w-wi) + vi, qui correspond à la valeur du ième élément à laquelle on ajoute la valeur du problème au rang i-1, pour une contenance de w-wi. En effet, on prend le ième élément pour le mettre dans notre sac, il prend donc de la place, il reste donc w-wi dans le sac.

Voilà le code correspondant :

``` python

L = [(1,2), (3,6), (1,4), (2,2), (4,1), (2,3)]
W = 5
KP = [[0 for j in range(W)] for i in range(len(L)+1)]


for i in range(1, len(L)+1):
  #on a un petit décalage ici, 
  wi, vi = L[i-1]
  
  for w in range(W):
    if wi > w:
      KP[i][w] = KP[i-1][w]
    else:
      KP[i][w] = max(KP[i-1][w], KP[i-1][w-wi] + vi)
      
  
print(KP)

```
Ce qui 






