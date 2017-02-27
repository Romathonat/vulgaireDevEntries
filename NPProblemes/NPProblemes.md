En informatique, et en ingenierie en général, on peut voir notre métier comme une resolution successive de problème. 
Créer un site web pour un client, mettre en place une architecture big data, résoudre des bugs d'affichage sur une IHM client lourd,
autant de problèmes que nous devons résoudre chaque jour. Finalement on est des spécialistes de la résolution de problème.

Alors attention, nous ne sommes pas capables de résoudre tous les problèmes, et on est même assez spécialisé :
un ingenieur réseau ne va pas, a priori, savoir calculer les contraintes mécaniques auquel est soumis un avion de ligne afin d'optimiser son poids. Il y a des catégories de problèmes.

Concernant l'informatique, c'est pareil. Il y a des catégories de problèmes, et celle qui m'interessent dans ce billet sont celles qu'on nomme **P, NP, NP-Complet, NP-difficile**. 

## Pourquoi classifier les problèmes ?
Pour savoir si on est capable de les résoudre ! On se rend bien compte qu'entre trouver, par exemple, le maximum d'un tableau, et le problème de la [somme de sous-ensembles](https://fr.wikipedia.org/wiki/Probl%C3%A8me_de_la_somme_de_sous-ensembles), il y a une différence ! 

Savoir qu'on est tombé sur un problème NP-complet, par exemple, nous permet de savoir qu'on ne pourra pas le résoudre sur des cas trop compliqués, car les meilleurs chercheurs de la planète s'y sont cassés les dents. Inutile donc de perdre trop de temps, avoir cette information peut permettre de mieux s'adapter à la situation, en proposant un algorithme qu'y s'approche de la bonne solution sans qu'on soit sûr qu'elle soit juste.

## Prérequis (informel)
Il faut savoir ce que c'est que la complexité d'un algorithme. L'explication mathématiques est un peu pompeuse, allez la chercher si vous voulez. En très gros, la complexité d'un algorithme c'est à quel point l'algorithme prend du temps, en fonction de la taille de son entrée. Par exemple trouver le maximum d'un tableau de taille n, on dit que sa complexité est de O(n) car pour le résoudre, on regarde n cases du tableau. 
Si on avait du parcourir deux fois le tableau, vous me direz qu'on noterait cela O(2n). Ca n'est pas faux, mais quand on varie seulement d'un facteur multiplicatif constant, on garde la notation O(n).

Maintenant, si on veut faire le [produit cartésien](http://www.bibmath.net/dico/index.php?action=affiche&quoi=./p/prodcart.html) du tableau avec lui-même, il faut itérer sur le tableau, et pour chaque itération réitérer sur tout le tableau pour trouver toutes les combinaisons possibles. On aura deux boucles imbriquées qui itèrent sur n, donc une complexité de O(n^2). Ca veut donc dire que le temps que va prendre mon algo dépend du carré de n, ce qui est moins bien que simplement de n.

Bon, je ne sais pas si j'ai été très clair, c'est une notion assez difficile à faire comprendre. Retenez que plus on a une complexité "simple", plus on est performant. Voici un par exemple quelques complexités, classées par ordre croissant:
- O(1) (temps constant, super)
- O(n)
- O(n^2)
- O(n!) (commence à devenir dur)
- O(2^n) (dur)
- O(n^n) (super dur)

Les complexités de la forme O(p^n), avec p constant, sont des complexités dites **polynomiales**.

## Les problèmes de classe P
Tous les problèmes pouvant être résolus en temps polynomial font parti de la classe P. 

