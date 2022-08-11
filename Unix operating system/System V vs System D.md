[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# systemD vs systemV
## Introduction 
 Les scripts dits « System V » sont ces scripts shell, situés sous /etc/init.d/ et dont l'appel avec les paramètres start ou stop produit les messages : Démarrage de .... [OK] ou Arrêt de ... [OK]. Ces scripts, ainsi que leur mécanique de lancement via /etc/rc, sont un héritage du vénérable Unix System V qui date de 1983. Cette architecture est simple, mais certains la trouvent lente, peu robuste et limitée, aussi depuis quelques années deux alternatives ont émergé au sein des principales distributions Linux. D'un côté, on trouve les partisans de Upstart (Ubuntu) et de l'autre les supporters de systemd (Fedora, Mandriva, OpenSuSE et bientôt RedHat et Debian). Nous allons dans ce wiki comparer la technologie de base présente dans le system V et celle qui vise à la remplacer, systemd. 
 
## Qu'est-ce que sont systemD et systemV ?

### Présentation system V

De son nom complet UNIX System V, System V est une version du système d'exploitation d'origine UNIX, dévoilée par l'entreprise AT&T en janvier 1983. UNIX étant une famille de systèmes d'exploitation multitâche et multi-utilisateur dérivé du Unix d'origine créé par AT&T. Beaucoup de système d'exploitation d'écoule directement de ce noyau c'est donc pourquoi il me semblait intéréssant de situer très rapidement ce qu'est UNIX. La première version de System V est sortie en 1983 et sa dernière mise à jour a eu lieu en 1997 avec la version release 5. System V est défini comme une version majeur de UNIX de part sa capacité de traitement rigoureux des accès aux données partagées.  

Voici un schéma permettant de visualiser schématiquement les différents OS Unix à travers le temps :
[SChéma](https://upload.wikimedia.org/wikipedia/commons/7/77/Unix_history-simple.svg)


### Présentation System D
systemd est une suite logicielle qui fournit une gamme de composants système pour les systèmes d'exploitation Linux. Systemd est le gestionnaire de système qui remplace upstart et son prédécesseur (les scripts system V) depuis Ubuntu 16.04 LTS Xenial. Le nom de ce programme vient de « system daemon » : le daemon du système. Il est le système d'init par défaut dans Debian depuis Debian 8 C'est une pièce maîtresse de l'architecture GNU/Linux. En effet, c'est le premier programme lancé par le noyau (il a donc le PID N°1) et il se charge de lancer tous les programmes suivants en ordre jusqu'à obtenir un système opérationnel pour l'utilisateur, selon le mode déterminé (single user, multi-user, graphique). C'est également à lui qu'incombe la tache de redémarrer ou arrêter votre ordinateur proprement. On peut donc résumé que systemd est le mécanisme d’initialisation de nombreuses. Systemd est une suite de blocs basiques pour construire un système Linux. Il fournit un gestionnaire de services et du système qui s'exécute en tant que PID 1 et démarre le reste du système. Systemd fournit des capacités de parallélisation intensives, utilise l'activation par sockets et D-Bus pour démarrer les services, offre un démarrage à la demande des daemons, garde la trace des processus en utilisant les groupes de contrôle de Linux, gère les points de montage et d'automontage, et implémente une logique élaborée de contrôle des services basée sur les dépendances transactionnelles. Systemd prend en charge les scripts d'initialisation SysV et LSB et fonctionne comme un remplacement de sysvinit.


## Différence entre Systemd et SystemV
Dans un souci de rationalisation et d'optimisation, systemd va réaliser des tâches qui étaient auparavant laissées au bon-vouloir du développeur des scripts System V. Par exemple, dorénavant, la gestion et le contrôle des PID des processus de service ainsi que ceux de leurs enfants sont à la charge de systemd. De même pour les logs. Systemd va aussi jouer le rôle de coordinateur et d'arbitre pour gérer dépendances et conflits.

Pour améliorer les performances du démarrage, systemd adopte plusieurs techniques astucieuses : tout d'abord, connaissant l'arbre de dépendances des services, il est naturellement capable de paralléliser les lancements de processus (on peut lancer A et B s'ils ne dépendent pas l'un de l'autre, même indirectement). Mais au-delà, systemd peut lancer en parallèle des processus interdépendants ! Comment cela ? Systemd va anticiper et créer une socket Unix pour tout service à démarrer. Ainsi, les « clients » d'un service (c'est à dire les services dépendants) vont se connecter dessus alors que le service réel n'est peut-être pas encore actif. Quand celui-ci le sera, systemd lui passera la socket, ainsi les « clients » bloqués pourront être servis !



## Pourquoi existe-t-il ces deux versions ?

## Comment ils font

## Comment utiliser
## Comment les utilisers

## Bibliographie

* [UNIX System V](https://fr.wikipedia.org/wiki/UNIX_System_V),  http://fr.wikipedia.org/w/index.php?title=UNIX_System_V&action=history, 1 février 2021 à 15:40, consulté le 10/08/2022
   - Résumé : ...
   - Avis sur la ressource : ... 
   
 * [systemd](https://fr.wikipedia.org/wiki/Systemd), http://fr.wikipedia.org/w/index.php?title=Systemd&action=history,3 août 2022,10/08/2022
* [systemd](https://doc.ubuntu-fr.org/systemd) Zarmu,12/07/2022,10/08/2022
* [systemD](https://wiki.debian.org/fr/systemd) unknown ,08/08/2022,10/08/2022
* [System V vs System D](https://www.quora.com/What-is-the-difference-between-SysVinit-and-systemd) unknown,2021, 10/08/2022
