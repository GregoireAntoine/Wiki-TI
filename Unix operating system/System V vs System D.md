[Accueil Wiki](https://epheclln.github.io/Wiki-TI/)
# systemD vs systemV
## Introduction 
 Les scripts dits « System V » sont ces scripts shell, situés sous /etc/init.d/ et dont l'appel avec les paramètres start ou stop produit les messages : Démarrage de .... [OK] ou Arrêt de ... [OK]. Ces scripts, ainsi que leur mécanique de lancement via /etc/rc, sont un héritage du vénérable Unix System V qui date de 1983. Cette architecture est simple, mais certains la trouvent lente, peu robuste et limitée, aussi depuis quelques années deux alternatives ont émergé au sein des principales distributions Linux. D'un côté, on trouve les partisans de Upstart (Ubuntu) et de l'autre les supporters de systemd (Fedora, Mandriva, OpenSuSE et bientôt RedHat et Debian). Nous allons dans ce wiki comparer la technologie de base présente dans le system V et celle qui vise à la remplacer, systemd. 
 
 
![Image](https://github.com/GregoireAntoine/Wiki-TI/blob/main/Assets/Images/image%20service%20lancement%20systemd%20vs%20systemV.PNG)


## Qu'est-ce que sont systemD et systemV ?

### Présentation system V

De son nom complet UNIX System V, System V est une version du système d'exploitation d'origine UNIX, dévoilée par l'entreprise AT&T en janvier 1983. UNIX étant une famille de systèmes d'exploitation multitâche et multi-utilisateur dérivé du Unix d'origine créé par AT&T. Beaucoup de système d'exploitation d'écoule directement de ce noyau c'est donc pourquoi il me semblait intéréssant de situer très rapidement ce qu'est UNIX. La première version de System V est sortie en 1983 et sa dernière mise à jour a eu lieu en 1997 avec la version release 5. System V est défini comme une version majeur de UNIX de part sa capacité de traitement rigoureux des accès aux données partagées.  

Voici un schéma permettant de visualiser schématiquement les différents OS Unix à travers le temps :
[SChéma](https://upload.wikimedia.org/wikipedia/commons/7/77/Unix_history-simple.svg)

### Présentation System D
systemd est une suite logicielle qui fournit une gamme de composants système pour les systèmes d'exploitation Linux. Systemd est le gestionnaire de système qui remplace upstart et son prédécesseur (les scripts system V) depuis Ubuntu 16.04 LTS Xenial. Le nom de ce programme vient de « system daemon » : le daemon du système. Il est le système d'init par défaut dans Debian depuis Debian 8 C'est une pièce maîtresse de l'architecture GNU/Linux. En effet, c'est le premier programme lancé par le noyau (il a donc le PID N°1) et il se charge de lancer tous les programmes suivants en ordre jusqu'à obtenir un système opérationnel pour l'utilisateur, selon le mode déterminé (single user, multi-user, graphique). C'est également à lui qu'incombe la tache de redémarrer ou arrêter votre ordinateur proprement. On peut donc résumé que systemd est le mécanisme d’initialisation de nombreuses. Systemd est une suite de blocs basiques pour construire un système Linux. Il fournit un gestionnaire de services et du système qui s'exécute en tant que PID 1 et démarre le reste du système. Systemd fournit des capacités de parallélisation intensives, utilise l'activation par sockets et D-Bus pour démarrer les services, offre un démarrage à la demande des daemons, garde la trace des processus en utilisant les groupes de contrôle de Linux, gère les points de montage et d'automontage, et implémente une logique élaborée de contrôle des services basée sur les dépendances transactionnelles. Systemd prend en charge les scripts d'initialisation SysV et LSB et fonctionne comme un remplacement de sysvinit.Systemd est compatible avec les scripts de démarrage SysV et LSB et à ce jour, il a réussi à remplacer SysVinit sur de nombreux Distros GNU / Linux., indépendamment des critiques valables ou des commentaires négatifs à son sujet.


## Différence entre Systemd et SystemV
Dans un souci de rationalisation et d'optimisation, systemd va réaliser des tâches qui étaient auparavant laissées au bon-vouloir du développeur des scripts System V. Par exemple, dorénavant, la gestion et le contrôle des PID des processus de service ainsi que ceux de leurs enfants sont à la charge de systemd. De même pour les logs. Systemd va aussi jouer le rôle de coordinateur et d'arbitre pour gérer dépendances et conflits.

Pour améliorer les performances du démarrage, systemd adopte plusieurs techniques astucieuses : tout d'abord, connaissant l'arbre de dépendances des services, il est naturellement capable de paralléliser les lancements de processus (on peut lancer A et B s'ils ne dépendent pas l'un de l'autre, même indirectement). Mais au-delà, systemd peut lancer en parallèle des processus interdépendants ! Comment cela ? Systemd va anticiper et créer une socket Unix pour tout service à démarrer. Ainsi, les « clients » d'un service (c'est à dire les services dépendants) vont se connecter dessus alors que le service réel n'est peut-être pas encore actif. Quand celui-ci le sera, systemd lui passera la socket, ainsi les « clients » bloqués pourront être servis !


Commandes relatives aux services :
Tâches / Système | SysVInit | Systemd	
---- | ---- | -----------
Démarrer un service  | service leservice start | systemctl start leservice.service 
Stopper un service|service leservice stop|systemctl stop leservice.service
Statut d'un service|service leservice status	|systemctl status leservice.service
vérifier si un service est activé au boot|chkconfig leservice|systemctl is-enabled leservice.service

Diverses commandes : 
Tâches / Système | SysVInit | Systemd	
---- | ---- | -----------
Arrêter le système|halt|systemctl halt
Eteindre le système|poweroff|systemctl poweroff
Redémarrer le système|reboot|systemctl reboot
Afficher les logs systèmes|tail -f /var/log/messages ou tail -f /var/log/syslog|journalctl -f

## Discussion à propos de Systemd dans la communauté 
Un partie de la communauté n'est pas fan du tout de systemd. En effet, il est classé comme lourd, complexe et possessif sur les Distros où il est implémenté, bien qu'il ait rempli de manière satisfaisante les objectifs pour lesquels il a été créé. À tel point que la célèbre Distro DEBIAN, la mère de nombreux autres Distros GNU / Linux, l'implémente depuis un certain temps, ce qui a contribué à sa massification.

## mythe à propos de systemd

### 1 Systemd est rapide 

Oui systemd D est considérer comme très rapide, un peu moisn de 1s pour le boot complet d'un espace utilisateur. Mais cette vitesse est due au fait que Systemd fait les choses correctement. En fait systemd est programmé sur le principe que j'appelle : la meilleure de code est celle qui n'existe pas. En effet, le code a été construit de façon à être le plus lisible possible. Et donc par conséquent, nous arrivons à un code avec peu de ligne, ou les chemins ne sont ni trop long ni inutiles et donc augmente ,sans le vouloir, la vitesse d'éxécution de ce code et donc la vitesse de boot d'un espace utilisateur

### 2 Systemd n'est pas compatible avec les scripts shell
Et bien c'est entièrement faux. Il ne sont just pas utiliser pendant la phase de démarrage car les conceveurs de systemd estime que ces scripts shells ne sont pas les meilleurs solutions pour cette tâche spécifique. 

Cependant vous pouvez exécuter n'importe quelle script en n'importe quel langage si vous le désirez.

### 3 Systemd est difficile 
Beaucoup de personne pense que Systemd est difficile à utiliser alors qu'en fait il est plus simple que les outils traditionnel linux car il unifie les objects du systeme et leurs dépendances.

Le langage du fichier de configuration est aussi très simple. 

systemd s'accompagne d'une courbe d'apprentissage. Mais c'est comme ça partout et pour tout. Cependant, il semble qu'il serait en fait plus simple de comprendre systemd qu'un démarrage basé sur Shell pour la plupart des gens.


## Conclusion
Pour conclure cet articles je dirais que systemd s'est "imposé", non pas partout, mais dans les distribs les plus populaires et les plus "grand public".

Cependant beaucoup d'utilisateurs plus confirmer dans le domaine de la programmation en reviennent et sans aller jusqu'au fork, de nombreuses versions alternatives (et non dérivées) de ces grosses distributions qui permettent d'éviter systemd apparaissent ou se désolidarisent de la distribution principale si elles existaient déjà. 

## Bibliographie

* [UNIX System V](https://fr.wikipedia.org/wiki/UNIX_System_V),  http://fr.wikipedia.org/w/index.php?title=UNIX_System_V&action=history, 1 février 2021 à 15:40, consulté le 10/08/2022
   - Résumé : ...
   - Avis sur la ressource : ... 
   
 * [systemd](https://fr.wikipedia.org/wiki/Systemd), http://fr.wikipedia.org/w/index.php?title=Systemd&action=history,3 août 2022,10/08/2022
* [systemd](https://doc.ubuntu-fr.org/systemd) Zarmu,12/07/2022,10/08/2022
* [systemD](https://wiki.debian.org/fr/systemd) unknown ,08/08/2022,10/08/2022
* [System V vs System D](https://www.quora.com/What-is-the-difference-between-SysVinit-and-systemd) unknown,2021, 10/08/2022
* [The Biggest Myths](http://0pointer.net/blog/projects/the-biggest-myths.html),Pid Eins,Unknown, 11/08/2022
* [Systemd vainqueur de Upstart et des scripts « System V » ?](https://connect.ed-diamond.com/GNU-Linux-Magazine/glmf-153/systemd-vainqueur-de-upstart-et-des-scripts-system-v) ,Delamarche Jérôme,octobre 2012,11/08/2022
* [Systemd contre Sysvinit.](https://blog.desdelinux.net/fr/systemd-versus-sysvinit-systemd-shim/) Unknown, 12/07/2019,11/08/2022
* [systemd] (https://wiki.archlinux.org/title/Systemd_(Fran%C3%A7ais)#:~:text=systemd%20est%20une%20suite%20de,d%C3%A9marre%20le%20reste%20du%20syst%C3%A8me.) Unknown,21 June 2022, 11/08/2022
* 
