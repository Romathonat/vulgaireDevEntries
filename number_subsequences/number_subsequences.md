
Après quelques temps d'inactivité, je reviens avec une nouvelle programmation dynamique.

## Les Séquences
Je travaille sur les motifs séquentiels en ce moment. Voyons quelques définitions.

Une **séquence** est une suite d'itemsets. Par exemple, si on considère mes achats au supermarché, l'ensemble de ce que j'ai acheté constitue un itemset: {carotte, vin blanc, pamplemousse} est un *itemset*. L'ordre des éléments n'a pas d'importance.

Si on considère maintenant l'ensemble des fois ou je suis venu au supermarché, cela constitue une séquence d'itemsets: <{carotte, vin blanc, pamplemousse}, {radis, yaourts}, {jus d'orange}> signifie que je suis allé aux supermarché trois fois différentes, que la première fois j'y ai pris des carottes, du vin blanc et du pamplemousse, la deuxième fois des radis et de yaourts, et la dernière fois du jus d'orange.

Les séquences ADN sont aussi des séquences, mais elles possèdent la particularité que chacun des itemset et constitué d'un seul élément (on appel cela un singleton). <{a}{c}{t}{g}{a}{a}> est une séquence. Les chaines de caractères sont aussi des séquences de singletons.

On dit qu'une séquence $$ s' = \langle X'_{1}, ..., X'_{m}\rangle $$ est une sous-séquence de $$s = \langle X_{1}, ..., X_{n}\rangle $$, notée: $$s'\sqsubseteq s$$ si il existe $$ 1 \leq j_{1} < j_{2} < ... < j_{m} \leq n $$ tel que $$X'_{1} \subseteq X_{j_{1}}, X'_{2} \subseteq X_{j_{2}}, ..., X'_{m} \subseteq X_{j_{m}}$$.   

C'est la définition formelle, intuitivement ça signifie qu'on peut prendre chaque itemset de s', dans l'ordre, et trouver un itemset de s dans lequel il est inclu. Par exemple:
s' = <{vin}, {carotte, chou}> est une sous-séquence de s = <{vin, coca}, {eau}, {carotte, huile, chou}>, parce que $$X'_1 \subseteq X_1 et X'_2 \subseteq X_3$$ et l'indice "3" de X_3 est "après" l'indice "1" de X_1.     
Par contre s'' = <{vin}, {carotte}, {chou}> n'est pas une sous-séquence de s.

## Problèmatique
**Combien existe-t-il de sous-séquences dans une séquence donnée s?**
On se place ici dans le cadre des sous-séquences de singleton, sinon c'est un peu plus compliqué même si l'idée reste la même (voir [ici](https://hal.inria.fr/hal-00740231v1/document), p5)

## La DP
Notons $$\phi(s)$$ l'ensemble des sous-séquences de s. Prenons un exemple, pour bien voir. Je vais enlever les acolades pour plus de lisibilité, et parce qu'on est dans le cas des itemsets singletons.

$$s = <h,o,n,o,l,u,l,u>$$ 

Pour énumérer les sous-séquences possibles, on ne prend que X_1 au départ (on note s<sup>1</sup> la séquence constituée uniquement de X_1). Ses sous-séquences sont: 

$$\phi(s^1) = \{<>, <h>\}$$
    
Ensuite on ajoute X_2. Les sous-séquences sont alors 
    
$$\phi(s^2) = \{ <>, <h>, <o>, <h,o>\}$$

En fait, à chaque nouvelle itération, on prend les sous-séquences du rang précédent, auxquelles on concatène un nouvel item. Il suffit donc de multiplier par deux le nombre de solutions du rang n-1.

... sauf qu'on va avoir un problème lorsque le nouvel item existe déjà dans la séquence précédente. Continuons:

$$\phi(s^3) = \{ <>, <h>, <o>, <h,o>, <n>, <h,n>, <o,n>, <h,o,n> \}$$
    
$$\phi(s^4) = \{ <>, <h>, <o>, <h,o>, <n>, <h,n>, <o,n>, <h,o,n>, <o,o>, <h,o,o>, <n, o>, <h,n,o>, <o,n,o>, <h,o,n,o>\}$$
    
Et là on se rend bien compte que le cas est différent, puisque le nombre d'éléments est inférieur au double du rang précédent. 
La récursivité pour la DP est donc la suivante:

$$
\phi(s^n) = \left\{
    \begin{array}{ll}
        \phi(s^{n-1}) \space \mbox{si } <X_n> \not\sqsubseteq s^{n-1}\\
        \phi(s^{n-1}) - R(s, X_n) \space \mbox{si } <X_n> \sqsubseteq s^{n-1}
    \end{array}
\right.
$$

Toute la difficulté est donc de calculer le terme R(s, Xn). Celui-ci permet de compter le nombre de sous-séquences qui ont déjà été énumérées dans les rangs précédents. Pour l'exemple qui a été utilisé, on se rend compte que de l'étape 3 à 4, on a pas réutilisé les séquences <> et < h >, car celles-ci auraient généré < o > et < h,o > qui sont déjà présentes. En fait, ce facteur correcteur R(s, X_n) correspond aux sous-séquences du rang n-1 qui sont déjà dans les solutions si on leur concatène Xn. Autrement dit, ce sont les **solutions du rang n-1 finissant par Xn, auxquelles on retranche Xn càd**: $$\phi(x^{l-1})$$ avec l l'indice du dernier itemset où est apparu X_n. Ceci n'est pas une vrai démonstration, mais ça devrait donner l'intuition. 

On a donc: 

$$
\phi(s^n) = \left\{
    \begin{array}{ll}
        \phi(s^{n-1}) \space \mbox{si } <X_n> \not\sqsubseteq s^{n-1}\\
        \phi(s^{n-1}) - \phi(x^{l-1}) \space \mbox{si } <X_n> \sqsubseteq s^{n-1}
    \end{array}
\right.
$$

## Implémentation

``` python
a = 'honolulu'
     
# solution are stored in key-value, with key being the rank in wich solutions
# have been found 
solutions = {0: 1}

for i, x in enumerate(a):
    if x not in a[:i]:
        solutions[i+1] = 2 * solutions[i]
    else:
        last_index = 0
        for j, char in enumerate(a[i-1::-1]):
            if char == x:
                last_index = j + 1
                break
 
        solutions[i+1] = 2 * solutions[i] - solutions[last_index - 1]    
         
print(solutions)
```

```
{0: 1, 1: 2, 2: 4, 3: 8, 4: 14, 5: 28, 6: 56, 7: 110, 8: 218}
```

Essayons de lancer maintenant sur une séquence ADN de taille 57, pour se rendre compte et compter le nombre de sous-séquences existantes:

```
57: 74344023495916820
```
On se rend bien compte que l'espace de recherche de données séquentielles peut être titanesque. 
