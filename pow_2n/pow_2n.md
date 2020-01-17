Vous développez une application quelconque. Certains éléments de cette application (des boutons, des barres de recherche, etc) peuvent être affichés ou non selon l'identité de l'utilisateur : il peut être admin, modérateur, ou utilisateur de base. Par exemple, l'admin voit tous les boutons existants : supprimer un article sur un blog, le modifier, l'ajouter, alors que l'utilisateur de base ne peut que consulter l'article.

La question est la suivante : "S'il y a n éléments dans ma fenêtre, combien de vues différentes sur ladite fenêtre puis-je avoir?"

Réponse : $$2^n$$

En effet, l'ensemble des possibilités, c'est l'ensemble des façons d'afficher un seul élément dans ma fenêtre, plus l'ensemble des façons d'en afficher deux etc. Ce raisonement nous mène à la formule suivante :  

$$\sum_{k=0}^{n}\binom{n}{k} = \sum_{k=0}^{n}\binom{n}{k}1^n1^{n-k} = (1 + 1)^n = 2^n$$

(d'après la formule du Binôme de Newton)

C'est juste, mais il est possible de trouver plus simplement.

Si on numérote les éléments dans cette fenêtre (pour les identifier par pour leur donner un ordre), et si on se rappelle que chaque élément a 2 états possibles, activé ou désactivé, alors on comprend vite qu'on est face à un mot binaire de n bits. Le nombre de possibilités vient ensuite très simplement puisque pour l'élement 1 on a deux possibilités, pour le 2 on a deux possibilités donc 2\*2 = 4 combinaisons possibles, pour le 3 on a deux possibilités aussi donc 4\*2 = 8 combinaisons possibles, etc. On trouve bien encore une fois 2^n possibilités.

On peut aussi démontrer ça par récurence, je ne fais pas la démo complète par soucis de concision, mais si au rang n on a 2^n possibilités, le fait d'ajouter un élément double les possibilités, car maintenant on a 2^{n} possibilités avec le nouvel élément activé, et 2^n possibilités sans l'élément activé, ce qui fait qu'on a bien 2^n+1 possibilités au rang n+1.

Le problème de découpe des barres.

Admettons que je travaille dans la metallurgie. Je souhaite gagner un maximum d'argent et je dispose d'une grande barre en acier, de taille n. J'ai aussi à ma disposition une scie, qui me permet de couper la barre à ma guise, avec pour limite que je ne coupe la barre que de sorte à avoir des morceaux dont les longueurs soient des entiers. Enfin, je possède un tableau qui me donne les cours du marché selon la longueur de la barre.

Comment est-ce que je coupe ma barre de sorte à maximiser mon profit?

On ne va pas résoudre le problème ici ce n'est pas le sujet, mais simplement calculer le nombre de possibilités de coupe.

Plutôt que de calculer le nombre de façons d'avoir des morceaux d'acier différents, il est plus simple de calculer le nombre de coupes differentes possibles, ce qui revient au même.

On revient donc exactement au même problème que précedemment, puisque le nombre de coupes différentes est égale au nombre de façons d'effectuer une seule coupe, plus le nombre de façons d'en faire deux etc, ce qui nous mène, comme nous l'avons vu précedemment à 2^n+1 possibilités.





