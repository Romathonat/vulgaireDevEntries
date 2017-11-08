## Préambule:
Comme l'article sur les probabilités, il s'agit ici d'un résumé de cours du MIT trouvable [ici](https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/readings/).
L'inférence Bayesienne est nouvelle pour moi, il est possible que certains éléments soient faux. Dans ce cas, et si vous le souhaites, vous pouvez me laisser un commentaire afin que je corrige l'article.

## Loi de Bayes
L'inférence bayésienne se base sur la loi de Bayes:
$$p(A|B) = \frac{p(B|A)p(A)}{p(B)}$$

## Estimateur du maximum de vraisemblance.
(Maximum Likelihood Estimates, MLE)
C'est une méthode permettant d'estimer la valeur d'un paramètre pour une distribution de probabilités données.  
Par exemple si on sait que la durée de vie d'une lampe se modélise bien par une loi géométrique, et qu'on a 5 ampoules qui ont vécues 2, 3, 1, 3 et 4 ans, on peut faire une estimation du paramètre lambda, grâce au MLE.

$$f(x1, x2, x3, x4, x5|\lambda) = (\lambda e^{-\lambda x1})(\lambda e^{-\lambda x2})(\lambda e^{-\lambda x3})(\lambda e^{-\lambda x4})(\lambda e^{-\lambda x5}) = \lambda^{5}e^{-\lambda(x1+x2+x3+x4+x5)}$$
$$f(2, 3, 1, 3, 4|\lambda) = \lambda^{5}e^{-13\lambda}$$

Le but est ensuite de trouver pour quelle valeur de lambda cette probabilité est maximale. Pour cela, on dérive la fonction selon lambda, on trouve pour quelle valeur elle s'annule, on vérifie qu'on est bien sur un max et pas un min. Par soucis de simplification, on préfère dériver le logarithme de la fonction.
Ici on a donc:
$$\frac{\partial ln(\lambda^{5}e^{-13\lambda})}{\partial \lambda} = \frac{5}{\lambda} - 13 = 0$$
$$\lambda = \frac{5}{13}$$


### Exemple
On a une classe de taille n, chaque étudiant possède un numéro unique. On en prend 3 au hasard, et on trouve les nombres 1, 3, 7. 
Trouver le maximum de vraisemblance. On considère que les numéros sont issus d'une distribution uniforme. 

La probabilité de tirer 3 numéros différents est de :
$$\frac{1}{\dbinom{n}{3}} = \frac{3!}{n(n-1)(n-2)}$$

car il n'existe qu'une combinaison qui sélectionne ces 3 éléments parmi n.
  
**NB:** On peut trouver cette probabilité autrement. On peut se dire que cette expérience revient à prendre un élément parmi n, puis un parmi n-1, puis un parmi n-2. 
Ceci nous donne une probabilité de :
$$\frac{1}{n(n-1)(n-2)}$$
En faisant cela, on a considéré la probabilité de tirer 1, puis 3, puis 7. Mais vu qu'on se moque de l'ordre, il faut aussi rajouter la possibilité de prendre 317, 731 etc, càd le nombre d'arrangement sans remise de k parmi k, soit 3! ici.

Finalement, il n'est pas vraiment nécessaire d'appliquer complétement la méthode du maximum de vraisemblance, puisque on voit directement que plus n est petit, plus la probabilité p(data|n) est forte, dans la limite où n ne peut pas être plus petit que 7. On a donc un MLE pour n = 7.


