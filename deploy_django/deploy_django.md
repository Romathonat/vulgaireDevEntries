Voici les prérequis :

- avoir un projet django fonctionel en local

- avoir un serveur auquel on a accès via ssh. Ça marche très bien avec un VPS aussi, ce qui ne coûte pas grand chose : de l'ordre de quelques euros par mois pour un serveur sur lequel vous pouvez installer ce que vous voulez + plusieurs Go d'espace, ça peut être vraiment intéressant (https://www.ovh.com/fr/vps/ par exemple).

- un peu de temps

Nous allons donc faire cette mise en prod, avec un serveur nginx + gunicorn. Connectons-nous via ssh au serveur.

```
ssh root@vps5469658.hebergeur.net
```

C'est parti.
Comment ça marche en gros

On va installer la triplette django+nginx+gunicorn sur le serveur. En gros nginx est le serveur HTTP qui se charge de recevoir les requêtes du même nom, va communiquer avec gunicorn, qui lui-même communique avec django via la norme wsgi.  Ci-dessous un schéma récapitulatif pompé chez sam et max (http://sametmax.com/quest-ce-que-wsgi-et-a-quoi-ca-sert/)

 

Schém d'utilistation de WSGI avec Nginx

 

En parallèle, on va installer une base de données MySQL, pour stocker nos données, avec laquelle django va communiquer. Pour une application plus importante où on souhaite répartir la charge, on conseille de ne pas avoir et la base de données et le serveur applicatif sur la même machine, mais ici on s'en contentera largement (laaargement).
Mettre son système à jour
```
sudo apt-get update
sudo apt-get upgrade
```
Installer MySQL

J'ai choisi MySQL comme base de donné, mais on peut tout aussi bien prendre MariaDB, PostgreSQL, Oracle etc.

On télécharge MySQL
```
sudo apt-get install mysql-server libmysqlclient-dev
```
Puis:
```
sudo mysql_install_db
```
Et enfin:
```
sudo mysql_secure_installation
```
Il faudra répondre à quelques simples questions, telles que définir un compte administrateur et son mot de passe.

On va maintenant créer un nouvel utilisateur pour la BD

On se log en tant qu'admin dans le shell MySQL:
```
mysql -u root -p
```
On crée la base de données relative à notre projet,
```
CREATE DATABASE monProjet CHARACTER SET UTF8;
```
puis on créé un nouvel utilisateur, qui aura les droits sur monProjet, et pas sur tout MySQL comme le compte admin précédemment créé
```
CREATE USER userMonProjet@localhost IDENTIFIED BY 'monMotDePasse';
```
On lui donne les droits (lecture, écriture, exécution) sur monProjet:
```
GRANT ALL PRIVILEGES ON monProjet.* TO userMonProjet@localhost;
```
Et on valide
```
FLUSH PRIVILEGES;
```
Puis on quitte le shell MySQL
```
exit
```
NB: Pour ce qui est de l'interfaçage avec django, voir la fin de ce tuto dans "masquer ses mots de passe"
Créer un nouvel User

Maintenant on va créer un nouvel utilisateur : les applications seront lancées sous sa juridiction. Ainsi, en cas de faille de sécurité, on peut espérer que l'attaquant ne puisse causer de dommages que là où ce nouvel utilisateur peut agir.
```
sudo useradd --system --shell /bin/bash --home /webApps webUser
```
(le dernier paramètre est le nom du nouvel user, et le --home définit le répertoire home, celui sur lequel on arrive en faisant "cd ~", de webUser)
Installer VirtualEnv et pip

VirtualEnv est un outil qui permet de créer un environnement virtuel Python. En gros, ça veut dire que toutes les installations python faites lorsqu'on est dans un environnement virtuel ne seront disponibles que dans cet environnement virtuel, et pas en-dehors. Ainsi, on peut avoir sur le même système plusieurs projets dont les versions de python, de django etc sont différentes. Un autre avantage est que si on se foire dans l'installation, ce n'est pas toute l'installation linux qui est compromise, mais simplement le virtualEnv ! (c'est assez pratique)

Mieux encore que VirtualEnv, nous allons installer virtualenvwrapper, qui est juste une surcouche nous permettant d'utiliser plus simplement les environnements virtuels.

Pip, quand à lui, est le gestionnaire de paquets de python (plus d'infos ici http://sametmax.com/votre-python-aime-les-pip/)

D'abord on installe des outils de bases pour python
```
sudo apt-get install python-setuptools
```
Parmi eux, se trouve easy_install, qui est comme pip mais en moins bien (ne gère pas la désinstallation des paquets par exemple : ca s'appelle easy_install pas easy_uninstall).

On l'utilise pour installer pip:
```
easy_install pip
```
Maintenant on installe virtualenvwrapper :
```
sudo pip install virtualenvwrapper
```
On va maintenant ecrire dans le .profile afin que le virtualenvwrapper se lance quand on se log dans linux:
```
vim ~/.profile
```
puis on écrit à la fin:
```
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```
On recharge le fichier de démarrage:
```
. ~/.profile
```
Enfin, il ne nous reste plus qu'à créer notre environnement virtuel :
```
mkvirtualenv virtualMonProjet --no-site-packages
```
--no-site-packages permet de n'avoir que python et pip dans son environnement. Pour se placer dans l'environnement virtuel, il suffit de faire:
```
workon virtualMonProjet
```
Vous verrez alors écrit (virtualMonProjet)root@monserveur dans la console. Si vous souhaitez sortir de l'environnement tapez "deactivate".

Attention pour la suite soyez bien dans votre environnement virtuel.
Installer Django et créer le dossier Projet

Ca devrait rouler tout seul étant donné ce qu'on a fait avant :
```
sudo pip install django
```
Maintenant on va choisir où mettre notre projet, on peut choisir de le mettre dans /webApps:
```
sudo mkdir -p /webapps
sudo chown webUser /webapps
```
Maintenant on va rapatrier notre projet depuis le local vers le serveur, en local, on tape:
```
scp -r monProjet root@serveur:/webApps
```
Et une fois sur le serveur on définit webUser comme propriétaire de ce dossier:
```
sudo chown webuser /webApps/monProjet/
```
 
## Installer Gunicorn

On va donc maintenant installer Gunicorn, qui va communiquer avec django comme on a vu dans le schéma plus haut.
```
sudo pip install gunicorn
```
Il faut configurer gunicorn dans un fichier avant de la lancer. On va le mettre dans /webApps/monProjet/bin
```
nano /webApps/monProjet/bin/gunicorn_start
```
Je vous file un fichier de config, reste juste à l'adapter:
```
#!/bin/bash
NAME="monProjet"                                  # Nom de l'application
DJANGODIR=/webapps/monProjet/            # Django project directory
USER=webUser                                        # le nom d'utilisateur
NUM_WORKERS=3                                     # garder le nombre de workers comme ca

echo "Start $NAME en tant que `whoami`"

# On active l'environnement virtuel
source /var/www/.virtualenvs/virtualVulgaireDev/bin/activate

#on active les variables d'environnement, présentes dans le .profile du home de webuser
source /webApps/.profile

# lance Django Unicorn
cd $DJANGODIR
exec gunicorn monProjet.wsgi \
  --name $NAME \
  --workers $NUM_WORKERS \
  --user=$USER \
```
Pour gérer gunicorn (restart, stop etc), on va utiliser supervisor. Celui-ci permet de relancer automatiquement les applications qu'on lui donne en cas de crash serveur, et de gérer des scripts (ici gunicorn_start).
```
sudo apt-get install supervisor
```
Pour spécifier un fichier de configuration pour supervisor relatif à gunicorn, il faut l'écrire dans /etc/supervisor/conf.d
```
sudo nano /etc/supervisor/conf.d/hello.conf
```
Puis on configure de cette manière ceci :
```
[program:monProjet]
command = /webapps/monProjet/bin/gunicorn_start ; Command to start app
user = webUser ; User to run as
stdout_logfile = /var/log/gunicorn/gunicorn.log ; Where to write log messages
redirect_stderr = true ; Save stderr in the same log
environment=LANG=en_US.UTF-8,LC_ALL=en_US.UTF- ; Set UTF-8 as default encoding
```
Puis on crée l'endroit où les logs doivent s'écrire :
```
mkdir -p /var/log/gunicorn
```
Enfin, on demande à supervisor de recharger ses fichiers de config.
```
sudo supervisorctl reread
```
Et on peut maintenant redémarrer gunicorn à notre guise :
```
sudo supervisorctl restart monProjet 
```
 
## Installer Nginx

Il ne reste plus qu'à installer le serveur Nginx, qui intercepte les requêtes HTTP pour les envoyer au couple (Gunicorn,Django)
```
sudo apt-get install nginx
sudo service nginx start
```
Si vous tapez votre nom de domaine, vous devriez arriver sur une page disant que nginx est présent.

On doit configurer nginx pour ce projet :
```
nano /etc/nginx/sites-available/monProjet
```
et encore une fois un fichier de configuration à adapter à votre projet:
```
# Configuration du server
server {
    listen      80;
    server_name www.monDomaine.fr monDomaine.fr;
    charset     utf-8;

    access_log /var/log/nginx/monProjet.access.log;
    error_log /var/log/nginx/monProjet.error.log;

    proxy_connect_timeout 300s;
    proxy_read_timeout 300s;

    # Fichiers media et statiques, délivrés par nginx directement
    location /media  {
        alias /webapps/monProjet/media;
    }

    location /static {
        alias /webapps/monProjet/static;
    }


    location / {
        proxy_pass http://127.0.0.1:8000; # Pass to Gunicorn
        proxy_set_header X-Real-IP $remote_addr; # get real Client IP

    }
}

 ```

Il faut maintenant faire en sorte que le site soit disponible, pour cela il faut créer un lien symbolique dans sites-enable:
```
sudo ln -s /etc/nginx/sites-available/monProjet /etc/nginx/sites-enabled/monProjet
```
Puis on peut redémarrer le service nginx.
```
sudo service nginx restart 
```
Maintenant ca devrait presque marcher, il faut juste ne pas oublier de mettre le debug = FALSE dans setting.py, et de bien faire l'interfaçage avec la BDD (juste après)
Masquer ses mots de passe

Avoir ses mots de passe directement écrits dans le setting.py c'est nul. On ne peut pas publier son code sur github ou autre sans que tout le monde les connaisse ce qui serait dramatique (ou alors on passe par un .gitignore), et franchement voir mes mots de passe écrits en dur comme ça, ça me met mal à l'aise.

Une solution est de les écrire dans le .profile, en bash, pour qu'à chaque fois qu'un utilisateur arrive, les mots de passe soient en variable d'environnement. On va écrire dans le .profile (si vous voulez plus d'explications sur le comment du pourquoi on écrit dans .profile et pas dans .bashrc http://superuser.com/questions/183870/difference-between-bashrc-and-bash-profile)
```
nano /home/webUser/.profile
```
Puis on ajoute ceci à la fin, ce qui va rendre cette variable globale au système linux (variable d'environnement):
```
export MONSITE_DB_PASSWORD='monMotdePasse'
```
Maintenant on ajoute une ligne à notre fichier gunicorn_start dans /webApps/MonProjet/bin/gunicorn_start, pour qu'à chaque fois que le serveur se lance il exécute .profile.
```
source /home/webUser/.profile
```
Enfin, il ne reste plus qu'à les récupérer dans le fichier setting.py, ici dans la connexion à la BD.
```

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'monProjet',
        'USER': 'adminMonProjet',
        'PASSWORD': os.environ.get("MONPROJET_DB_PASSWORD", ''), #permet de chopper la variable d'environnement
        'HOST': 'localhost',
        'PORT': '',
    }
}
```
 
Utiliser git pour envoyer son projet sur le serveur

On utilise git pour versionner son projet, et/ou pour travailler à plusieurs le plus souvent. Mais ce qui est cool c'est qu'on peut aussi l'utiliser pour envoyer notre projet sur le serveur. Vous faites vos modifications en local, tout marche bien, vous faites un commit, et bam vous balancez tout sur la prod, rien de plus à faire.

On commence par créer un repo git un peu particulier : un --bare, qui ne contient aucun fichier source 
```
sudo mkdir -p /var/repo/monProjet.git
cd /var/repo/monProjet.git
git init --bare
```
Maintenant que c'est fait, vous verrez un dossier hook dans le dossier monProjet.git. On peut y définir des fichiers qui vont intercepter les push, et faire des actions. Ici on va définir un fichier qui intercepte les push de l'utilisateur, et rediriger ce qui a été push non pas vers /var/repo/monProjet.git, mais vers /webApps/monProjet
```
vim hooks/post-receive
```
puis on écrit dedans:
```
#!/bin/sh
git --work-tree=/webApps/monProjet --git-dir=/var/repo/monProjet.git checkout -f
```
"--git-dir" spécifie le repertoire source, "--work-tree" spécifie l'endroit vers lequel on veut rediriger le push. On remarque que c'est encore un checkout (git utilise souvent cette commande) : ici il ne signifie pas "changer de branche", mais "mettre à jour l'espace de travail"

On ajoute les droits d'execution :
```
chmod +x hooks/post-receive
```
Maintenant, on se met sur la machine local, et on init un repo git dans notre dossier
```
cd /chemin/vers/projet/monProjet
git init
```
Et maintenant, on ajoute une "remote" à notre repo :
```
git remote add prod ssh://user@mydomain.com/var/repo/monProjet.git
```
Et voilà, à chaque fois qu'on voudra envoyer notre projet en prod, on fera :
```
git push prod master
```
(bien sûr ca ne push que les commit, ça reste un dépôt git hein)
Pour finir

J'espère que le tuto vous aura permis de bien vous débrouiller, et que vous saurez résoudre vos problèmes, car soyons sérieux, si vous n'êtes pas habitués à effectuer des mises en prod, il y en aura surement. Méfiez-vous notamment des droits linux sur les dossiers et fichiers, parfois vous ne les avez pas et ça plante.

Bon courage!

 

Sources :

[http://michal.karzynski.pl/blog/2013/06/09/django-nginx-gunicorn-virtualenv-supervisor/]

[https://www.digitalocean.com/community/tutorials/how-to-set-up-automatic-deployment-with-git-with-a-vps]

[https://www.digitalocean.com/community/tutorials/how-to-use-mysql-or-mariadb-with-your-django-application-on-ubuntu-14-04]

[http://sametmax.com/votre-python-aime-les-pip/]
