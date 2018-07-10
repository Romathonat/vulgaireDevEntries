
Après quelques temps d'inactivité, je reviens avec une nouvelle programmation dynamique.

## Les Séquences
Je travaille sur les motifs séquentiels en ce moment. Voyons quelques définitions.

Une **séquence** est une suite d'itemsets. Par exemple, si on considère mes achats au supermarché, l'ensemble de ce que j'ai acheté constitue un itemset: {carotte, vin blanc, pamplemousse} est un *itemset*. L'ordre des éléments n'a pas d'importance dans un itemset.

Si on considère maintenant l'ensemble des fois ou je suis venu au supermarché, cela constitue une séquence d'itemsets: <{carotte, vin blanc, pamplemousse}, {radis, yaourts}, {jus d'orange}> signifie que je suis allé aux supermarché trois fois différentes, que la première fois j'y ai pris des carottes, du vin blanc et du pamplemousse, la deuxième fois des radis et de yaourts, et la dernière fois du jus d'orange.

Les séquences ADN sont aussi des séquences, mais elles possèdent la particularité que chacun des itemset et constitué d'un seul élément (on appel cela un singleton). <{a}{c}{t}{g}{a}{a}> est une séquence. Les chaines de caractères sont aussi des séquences de singletons.

On dit qu'une séquence $ s' = \langle X'_{1}, ..., X'_{m}\rangle $ est une sous-séquence de $s = \langle X_{1}, ..., X_{n}\rangle $, notée: $s'\sqsubseteq s$ si il existe $ 1 \leq j_{1} < j_{2} < ... < j_{m} \leq n $ tel que $X'_{1} \subseteq X_{j_{1}}, X'_{2} \subseteq X_{j_{2}}, ..., X'_{m} \subseteq X_{j_{m}}$.   
C'est la définition formelle, intuitivement ça signifie qu'on peut prendre chaque itemset de s', dans l'ordre, et trouver un itemset de s dans lequel il est inclu. Par exemple:
s' = <{vin}, {carotte, chou}> est une sous-séquence de s = <{vin, coca}, {eau}, {carotte, huile, chou}>, parce que $X'_1 \subseteq X_1$ et $X'_2 \subseteq X_3$ et l'indice "3" de $X_3$ est "après" l'indice "1" de $X_1$.   
Par contre s'' = <{vin}, {carotte}, {chou}> n'est pas une sous-séquence de s.

## Problèmatique
**Pour une séquence s, combien existe-t-il de sous-séquences ?**
On se place ici dans le cadre des sous-séquences de singleton, sinon c'est un peu plus compliqué même si l'idée reste la même (voir [ici](https://hal.inria.fr/hal-00740231v1/document), p7)

## La DP
Notons $\phi(s)$ l'ensemble des sous-séquences de s. Prenons un exemple, pour bien voir. Je vais enlever les acolades pour plus de lisibilité, et parce qu'on est dans le cas des itemsets singletons.
$s = <h,o,n,o,l,u,l,u>$ Pour énumérer les sous-séquences possibles, on ne prend que $X_1$ au départ (on note $s^1$ la séquence constituée uniquement de $X_1$). Ses sous-séquences sont $\phi(s^1) = \{<>, <h>\}$. Ensuite on ajoute $X_2$. Les sous-séquences sont alors $\phi(s^2) = \{ <>, <h>, <o>, <h,o>\}$. En fait, à chaque nouvelle itération, on prend les sous-séquences du rang précédent, auxquelles on concatène nouvel item. Il suffit donc de multiplier par deux le nombre de solutions du rang n-1.

... sauf qu'on va avoir un problème lorsque le nouvel item existe déjà dans la séquence précédente. Continuons:
$$\phi(s^3) = \{ <>, <h>, <o>, <h,o>, <n>, <h,n>, <o,n>, <h,o,n> \}$$
$$\phi(s^4) = \{ <>, <h>, <o>, <h,o>, <n>, <h,n>, <o,n>, <h,o,n>, <o,o>, <h,o,o>, <n, o>, <h,n,o>, <o,n,o>, <h,o,n,o>\}$$
Et là on se rend bien compte que le cas est différent.
