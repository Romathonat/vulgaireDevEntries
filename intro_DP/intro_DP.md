## Intro
Dynamic Programming. Ou programmation dynamique en français. C'est une technique de résolution de problèmes, qui peut être un peu compliquée à comprendre. 

Le principe de la programmation dynamique est d'utiliser la récursivité d'un problème, on parle de **sous-structure optimale**.
Cela consiste à prendre un problème de taille N, le résoudre pour les versions de taille moindre, de la plus petite à la plus plus grande,
en stockant les résultats intermédiaires (un genre de [mémoïsation](https://fr.wikipedia.org/wiki/M%C3%A9mo%C3%AFsation)).

Alors bien sûr, on ne peut pas utiliser la programmation dynamique pour résoudre tous les problèmes, ce serait trop simple. Il faut
que celui-ci présente une sous-structure optimale.

## Le problème du sac à dos
Prenons un bon exemple pour mieux comprendre. Vous possédez un sac à dos qui peut contenir un poids maximal W. Vous possédez i objets, que vous pouvez mettre dans le sac à dos, chacun ayant un poids wi et une valeur vi. Le but est de trouver l'ensemble des objets i qui maximise la somme des valeurs que vous avez dans votre sac, sans dépasser le poids W.  

On pourrait se dire "c'est facile, il suffit de commencer par mettre les objets les plus lourds, puis les plus légers et ça marche".  

*Contre exemple*: W = 5, objets = {(5, 2), (2, 3), (2, 3)} (le premier terme est le poids, le second est la valeur).
En applicant cela, on se retrouve avec un seul objet de poids 5, pour une valeur de 2, alors que si on avait mis les deux objets de poids 2, on aurait une valeur totale de 4, ce qui aurait été mieux.

On pourrait aussi se dire "bon ben alors, il faut commencer par mettre les objets de plus fortes valeurs en premier".
    
*Contre-exemple*: W = 5, objets = {(5,4), (2,2), (1,3)}

Pareil, on se retrouverait avec une valeur totale de 4 avec un objet de poids 5, alors que mettre les deux objets (2,2) et (1,3) auraient constitué une valeur totale de 5.

En fait ces deux algorithme sont des algorithmes **gloutons**, c'est à dire qu'à chaque étape on fait un choix qui sur le coup nous 
paraît le meilleur, mais sur le long terme il ne l'est pas forcément. Par exemple, si on considère que notre problème est le bien de l'humanté, utiliser des énergies fossiles peut paraître être un bon choix localement dans le temps, puisque ça permet l'air industriel, le développement économique et technique, par contre sur le long terme, ce n'est pas la solution optimale puisque ça nous mène à des problèmes climatiques (entre autres) qui sont néfastes à l'humanité :)

Si on regarde l'espace des solutions possibles, on se rend vite compte que pour n objets, il y a 2^n possibilités. En effet, on peut associer ces solutions à un mot de n bits, chaque bit représentant si on met l'objet dans le sac ou pas (si vous ne comprenez pas pourquoi, aller regarder [le premier article de ce blog](http://vulgairedev.fr/blog/article/2-puissance-n).

En fait, on est face à un problème [NP-complet](http://vulgairedev.fr/blog/article/problemesP_NP). On peut cependant se débrouiller avec un algorithme dit [pseudo-polynomial](https://en.wikipedia.org/wiki/Pseudo-polynomial_time), en 
programmation dynamique !

## Solution

Notre problème dépend de deux paramètres: les éléments que l'on met dans le sac, ainsi que la contenance du sac. Notons-le KP(i, w)(KP pour Knapsack Problem), avec i signifiant qu'on prend les i premiers objets. KP(i, w) nous donne la **somme maximale des valeurs maximales qu'on puisse avoir**.

On peut voir que dans ce problème du sac à dos, on a une sous-structure optimale.
En effet, on a:

**si wi > w** : KP(i, w) = KP(i-1, w)  
**sinon**: KP(i, w) = max(KP(i-1, w), KP(i-1, w-wi) + vi) 

Bon, c'est peut être pas ce qu'il y a de plus compréhensible écrit comme ça. J'essaie de l'expliquer mais c'est assez difficile.
En fait, ça veut dire que quand on prend le problème au rang i, on a deux cas:
- Soit le ième élément a un poids wi supérieur à w, du coup on ne peut pas le mettre dans le sac. Du coup pour avoir la solution au problème, il suffit de regarder quelle est la valeur de KP au rang i-1 avec la même contenance w.
- Soit le ième élément peut être mis dans le sac. Dans ce cas, il faut prendre la valeur maximale entre KP(i-1, w), qui est la valeur du problème au rang i-1 (sans prendre le iéme élément), et KP(i-1, w-wi) + vi, qui correspond à la valeur du ième élément à laquelle on ajoute la valeur du problème au rang i-1, pour une contenance de w-wi. En effet, on prend le ième élément pour le mettre dans notre sac, il prend donc de la place, il reste donc w-wi dans le sac.

Voilà le code correspondant :

``` python

L = [(1,2), (3,6), (1,4), (2,2), (4,1), (2,3)]
W = 5
KP = [[0 for j in range(W+1)] for i in range(len(L)+1)]


for i in range(1, len(L)+1):
  wi, vi = L[i-1]
  
  for w in range(W+1):
    if wi > w:
      KP[i][w] = KP[i-1][w]
    else:
      KP[i][w] = max(KP[i-1][w], KP[i-1][w-wi] + vi)
      
  
print(KP)

```
>
[0, 0, 0, 0, 0, 0]  
[0, 2, 2, 2, 2, 2]  
[0, 2, 2, 6, 8, 8]  
[0, 4, 6, 6, 10, 12]  
[0, 4, 6, 6, 10, 12]  
[0, 4, 6, 6, 10, 12]  
[0, 4, 6, 7, 10, 12]    
pour la dernière ligne de KP. Ceci veut dire que pour des contenances de 0, 1, 2, 3, 4 et 5, on a des sommes de valeurs maximales de 0, 4, 6, 7, 10 et 12.

Pour connaitre la composition des solutions, on peut faire un tableau de booléens, de même taille que KP, qui retient les valeurs que l'on a choisi. Ainsi, on peut reconstruire la solution:

``` python
L = [(1,2), (3,6), (1,4), (2,2), (4,1), (2,3)]
W = 5
KP = [[0 for j in range(W+1)] for i in range(len(L)+1)]
keep = [[0 for j in range(W+1)] for i in range(len(L)+1)]

for i in range(1, len(L)+1):
  #on a un décalage car i commence à 1 et fini à len(L)+1
  wi, vi = L[i-1]
  
  for w in range(W+1):
    if wi > w or KP[i-1][w-wi] + vi < KP[i-1][w]:
      KP[i][w] = KP[i-1][w]
    else:
      KP[i][w] = KP[i-1][w-wi] + vi
      
      #à chaque fois qu'on prend le ième élément, on sauvegarde dans le tableau keep
      keep[i][w] = 1

#K est une valeur qui va nous permettre de retracer le chemin qui a été pris dans la DP.
#Etant donné que la solution de notre problème est en KP[N][W], on fait prendre à K la valeur W.
K = W
result = []

#Ensuite, on part de la ligne du bas du tableau (càd avec tous les objets pris en compte), et on cherche quand un objet a été ajouté
for i in range(len(L), 0, -1):
  #à chaque fois qu'on trouve un 1, cela signifie que l'objet est a été ajouté à cette étape
  if(keep[i][K] == 1):
    result.append(i - 1)
    
    #si l'objet a été ajouté, cela signife qu'on a utilisé une solution du sous probleme KP[i-1][w-wi], donc il faut adapter K
    #en lui faisant prendre la valeur w-wi
    K = K - L[i-1][0]

#on affiche les indices des objets qui sont utilisés
print(result)
```
>[2, 1, 0]

Voilà, j'espère que cet article vous aura plu. C'est un sujet un peu délicat donc n'hésitez pas à lire cela au calme. 






