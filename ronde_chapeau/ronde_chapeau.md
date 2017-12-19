Je vous propose la petite énigme suivante :


Dans une salle sans miroir se trouvent N personnes. Chacune de ces personnes porte sur la tête un chapeau coloré et peut voir la couleur des chapeaux des autres personnes. Par contre, une personne ne peut pas connaitre la couleur de son chapeau.

On sait qu'il existe N couleurs de chapeau différentes. Attention, plusieurs personnes peuvent avoir un chapeau de même couleur sur la tête! Ainsi, une couleur de chapeau qui existe peut ne pas être portée. (Il n'y a pas de bijection entre les couleurs et les personnes).

Au bout d'un certain temps, les N personnes vont annoncer simultanément une couleur. Quelle stratégie doivent-elles mettre en place pour qu'au moins l'une d'entre elle annonce la couleur de son chapeau?


La solution est la suivante : (N'oubliez pas de chercher par vous même auparavant !)


Afin de simplifier l'énigme, on peut considérer un problème équivalent dans lequel les chapeaux ne sont pas colorés, mais numérotés de 0 à N-1 (un numéro par couleur). Ainsi, chaque personne porte sur sa tête un numéro qu'il va devoir deviner.

Remarquons que la somme des numéros des chapeaux modulo N donne un nombre inclus dans l'intervalle [0 ; N-1]. Nous avons à disposition autant de personnes que de sommes (modulo N) possibles. On décide alors d'associer à chaque personne une somme possible. Par exemple, la personne k sera associée à la somme possible k. Je propose d'appeler S la somme réelle des chapeaux (modulo N).

Une fois ceci posé, la stratégie à mettre en place est la suivante : chaque personne doit calculer la somme des chapeaux visibles (modulo N). Cette somme intermédiaire Si est égale à

Dans cette équation, la personne connait Si, mais pas S ni la valeur de son propre chapeau. Cependant, nous avons associé chaque personne à une somme possible k. On va donc demander à chaque personne de supposer que S = k. L'équation ci dessus devient :

Par conséquent, chaque personne peut calculer une valeur pour chapeau de k. Cette valeur sera la véritable valeur du chapeau de k, à l'unique condition que :

Or cette condition est vraie pour exactement une personne dans la salle ! En effet, il y a N personnes, et chacune possède une somme possible k différente, on tombera donc forcément sur quelqu'un qui aura S=k.


En résumé : chaque personne reçoit un nombre k, calcule Si égal à la somme des chapeaux visibles, puis annonce k - Si (modulo N).

NB : comme nous sommes dans le cas d'un modulo, il peut s'avérer possible que :

Dans ce cas là, il ne faut pas annoncer k - Si, puisqu'on aurait un résultat négatif. Il faut annoncer k + N - Si.


Pour que ce soit bien clair, voici un exemple :

Prenons le cas où il y a 4 personnes. Elles portent des chapeaux numérotés. Voici la distribution des chapeaux :
Personne (k)	Chapeaux
0	3
1	2
2	3
3	1


La personne 0 va supposer que la somme des chapeaux modulo 4 vaut 0. Elle calcule la somme des chapeaux visibles : 2+3+1 = 6.

Donc :

Elle doit donc annoncer k - Si, soit 0 - 2. On se trouve exactement dans le cas k < Si. Donc au lieu d'annoncer -2, elle annonce k - Si + N, soit 0 - 2 + 4, à savoir 2.


Passons cette fois à la personne 1. La somme visible est 3+3+1 = 7.

Si = 3 (mod 4)

Elle annonce k - Si + N, soit 1 - 3 + 4 = 2.

La personne 1 a vu juste !



