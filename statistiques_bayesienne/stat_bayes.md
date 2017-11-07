### Exemple
On a une classe de taille n, chaque étudiant possède un numéro unique. On en prend 3 au hasard, et on trouve les nombres 1, 3, 7. 
Trouver le maximum de vraisemblance. On considère que les numéros sont issus d'une distribution uniforme. 

La probabilité de tirer 3 numéros différent est de :
$$\frac{1}{\dbinom{n}{3}} = \frac{3!}{n(n-1)(n-2)}$$

car il n'existe qu'une combinaison qui sélectionne ces 3 éléments parmi n.
  
**NB:** On peut trouver cette probabilité autrement. On peut se dire que cet expérience revient à prendre un élément parmi n, puis un parmi n-1, puis un parmi n-2. 
Ceci nous donne une probabilité de :
$$\frac{1}{n(n-1)(n-2)}$$
En faisant cela, on a considéré la probabilité de tirer 1, puis 3, puis 7. Mais vu qu'on se moque de l'ordre, il faut aussi rajouter la possibilité de prendre 317, 731 etc, càd le nombre d'arrangement sans remise de k parmi k, soit k! .
