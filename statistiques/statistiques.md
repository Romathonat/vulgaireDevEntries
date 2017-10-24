Je suis en train de faire les cours du MIT sur les probabilités/statisitques ([ici](https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/readings/)). Le titre officiel est "Introduction 
aux probabilités et statistiques, mais le cours est tout de même assez complet je trouve, donc je fais un petit résumé ici (ce n'est pas un cours, donc si vous ne connaissez pas un minimum ça risque d'être un peu dur).

## Loi de multiplication
*S'il y a n façons de réaliser l'actions 1, et m façons de réaliser l'action 2, alors il y a n*m façons de réaliser l'action 1 suivit de l'action 2*.
**Exemple:** Combien y a-t-il de façons de tirer une paire d'as si l'on tire deux cartes d'un jeu de 52 cartes ?
**Réponse:** Il y a 4 as dansle jeu, donc 4 possibilités pour le premier tirage. Une fois qu'un premier as a été tiré, il n'en reste plus que 3 pour le second tirage, on a 3 possibilités. On a donc 4\*3=12 tirages possibles. D'après la loi qui suivante, on a donc une probabilité de:
$$\frac{12}{52*51} = \frac{1}{221}$$

Avoir une paire d'as avant le flop au poker est donc rare, puisqu'on a seulement 0.45% de chances.

## Dénombrement
Dans le cas discret (par exemple tirer au hasard des cartes parmis un jeu de 52 cartes), si il y a équiprobabilité des issues, on fait souvent:
$$\frac{nombre\_issues\_qui\_nous\_interessent}{nombre\_issues\_possibles}$$

Par exemple, trouver la probabilité de tirer un carreau:
$$\frac{13}{52} = \frac{1}{4}$$

### Arrangement sans répétition
C'est un arrangement, on le note:  
$$A_n^{k}$$

Un arrangement avec répétion c'est prendre k billes parmis n dans un sac de billes, en tenant compte de l'ordre dans lequel on sort chacune d'elle. L'ensemble des issues (qui sont de longueur k) correspond aux arrangements sans répétition.

Par exemple j'ai un sac avec une boule verte (V), une bleue (B) et une rouge(R) (n=3), dans lequel je prend deux billes (k=2) l'ensemble des issues possibles est:
$${VB, VR, BV, BR, RV, RB}$$   

Le nombre d'issues possibles est:

$$\frac{k!}{(n-k)!}$$


### Arrangement avec répétition


### Combinaison sans répétition


### Combinaison avec répétition






