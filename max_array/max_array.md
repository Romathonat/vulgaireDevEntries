Vous disposez d'un tableau contenant des nombres, positifs ou négatifs. On vous donne 2 nombres A et B.

Quelle est la somme d'éléments consécutifs commençant par A et finissant par B (compris) qui est maximale ?

Exemple :

``` python
A = 2, B = 5, tableau =  [1, 2, -6, 4, 5, -2, 2, -8, 5]
``` 
Le résultat maximal sera 5, entre les index 1 et 4.

Comment résoudre le problème ?

Une solution naïve serait de parcourir le tableau, puis si on tombe sur un A, on parcourt tout le sous-tableau à droite en sommant et en prenant le max lorsque l'on tombe sur un B.

``` python
t = [1,2,-6,4,5,-2,2,-8,5]
a = 2
b = 5

#on initialise à - l'infini
result = -float("inf")

for i,elt in enumerate(t):
    if elt == a:
        count = 0
        for elt2 in t[i:]:
            count += elt2
            if elt2 == b:
                result = max(result, count)

print(result)
``` 

On voit bien qu'ici on a une complexité en O(n2). On peut mieux faire. Le code suivant est plus efficace, mais moins intuitif à trouver (pour moi en tout cas).

On va dire qu'on utilisera seulement 2 variables, result et somme, initialisées à - l'infini. A chaque fois qu'on avance dans le tableau, on ajoute la valeur courante à somme. Si on tombe sur un a, on vérifie que la somme est bien supérieure à 0. Si ce n'est pas le cas, alors on la recommence avec ce nouveau a. En effet, la somme des valeurs du sous-tableau précédent est négative, elle fait "perdre de la valeur", donc on la "coupe" de notre tableau courant, càd on recommence la somme à partir de ce nouveau a.

Petite originalité intéressante de python, on peut définir une valeur à l'infinie comme ceci :

``` python
a = float("inf")
``` 

Cet objet se comporte comme la notation de l'infini en math : si on lui retire une valeur finie ça reste l'infini, si on fait l'infini-l'infini on obtient NaN (Not a Number), càd que ce n'est pas défini, etc.

Voilà le nouveau code :

``` python
t = [1,2,-6,4,5,-2,2,-8,5]
a = 2
b = 5

#on initialise à - l'infini
result = -float("inf")
somme = -float("inf")

for elt in t:
    if elt == a and somme < 0:
        somme = a
    else:
        somme += elt
    
    if elt == b:
        result = max(result, somme)
   
print(result)
``` 

Et du coup, on peut facilement résoudre le problème du maximum de la somme des termes consécutifs dans un tableau, sans restriction (pas de A et de B) :

``` python
t = [1,2,-6,4,5,-2,2,-8,5]

#on initialise à - l'infini
result = -float("inf")
somme = -float("inf")
 
for elt in t:
    if somme < 0:
        somme = elt
    else:
    	somme += elt
 
    result = max(result, somme)
print(result)
``` 

