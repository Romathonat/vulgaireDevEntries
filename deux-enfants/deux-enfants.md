# L'énigme des deux enfants

Je suis tombé sur l'énigme des deux enfants, proposé par [science4all](https://www.youtube.com/channel/UC0NCbj8CxzeCGIF6sODJ-7A/featured).

## Problème
Un couple a deux enfants. L'un de ces enfants est un garçon. Quelle est la probabilité pour que l'autre enfant soit aussi un garçon ?

On fait les hypothèses que le sexe d'un enfant n'est pas influencé par le sexe de l'enfant né précédemment (=évènements indépendants), et que on a une une chance sur deux d'avoir un garçon.

Intuitivement on voudrait dire une chance sur deux. Nous allons voir que non.

## Solution mathématique
On peut résoudre ce problème par la formule de Bayes. On pose A = "Au moins un enfant est un garçon" et B = "Le deuxième enfant est un garçon". On veut donc savoir la probabilité d'avoir un garçon sachant qu'on a au moins un garçon parmi ces deux enfants, soit p(B|A)
$$ p(B|A) = \frac{p(A|B)*p(B)}{p(A)} $$

On a  p(A) = 3/4, car il a 4 cas possibles équiprobables : Soit le couple a eu un garçon puis une fille, soir une fille puis un garçon, soir deux garçons, soit deux filles.  
On a p(B) = 1/2. Dans l'absolu, sans avoir d'informations supplémentaire, on a une chance sur deux d'avoir un nouvel enfant qui est un garçon.  
On a p(A|B) = 1. Si un enfant est un garçon, alors obligatoirement au moins un enfant est un garçon.  

On a donc p(B|A) = 1/3 (et non pas 1/2 comme on pouvait le penser initialement).

En fait ce qui est bien important de comprendre c'est que l'énonce ne dit pas "le couple a eu un garçon, *puis* un autre", mais "Le couple a deux enfants, *dont* un garçon". C'est cette information qui change tout: on se retrouve avec les possibilités GG, FG et GF, alors que dans le premier cas on aurait pu avoir que GG et GF.

## Expérience
Testons par l'expérience "in silico". 

``` python
import random

# SIMULATION: pleins de couples avec deux enfants
couple_nombre = 10000
enfants = []
enfants_possibles = ['F', 'G']

for i in range(couple_nombre):
  # le couple fait un premier enfant:
  premier_enfant = random.sample(enfants_possibles, 1)[0]

  # le couple fait un deuxieme enfant:
  deuxieme_enfant = random.sample(enfants_possibles, 1)[0]

  enfants.append([premier_enfant, deuxieme_enfant])

# EXPERIENCE
# On va tester plein de fois l'experience suivante : on prend un couple qui a un garçon parmi deux enfants, et on regarde si le deuxième est un garçon.

# on filtre maintenant les données pour obtenir tous les couples où on a au moins un garçon
enfants_garcon = list(filter(lambda x: x[0] == 'G' or x[1] == 'G', enfants))


xp_nombre = 5000
nombre_soeurs = 0
nombre_freres = 0

for i in range(xp_nombre):
  # on prend un de ces couple d'enfants où il y a un garçon:
  couple_enfants = random.sample(enfants_garcon, 1)[0]

  # on test si on a un garçon où un fille
  if couple_enfants[0] == 'F' or couple_enfants[1] == 'F':
    nombre_soeurs += 1
  else:
    nombre_freres += 1

print('La probabilité empirique d\'avoir un garçon est de {}'.format(nombre_freres/xp_nombre))
```
Résultats
``` bash
La probabilité empirique d\'avoir un garçon est de 0.3368
```

On applique ici la loi des grands nombres : la moyenne de n évènements qui suivent la même loi de probabilité converge vers l'espérance du phénomène. On trouve que pour n = 5000 couples, en moyenne on a 0.3368 chances d'avoir un garçon, ce qui correspond bien au 1/3 trouvé précédemment.
