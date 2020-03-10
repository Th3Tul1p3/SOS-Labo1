# Rapport SOS Labo 1

**Auteur : ** Jean-Luc Blanc et Jérôme Arn 

**Date : ** 2 mars 2020 

# Q 1 : Expliquer l’utilité de l’argument -Pn et dans quelles circonstances est-ce qu’il s’utilise?

L'argument -Pn permet de considérer tous les hôtes comme étant connectés, cela permet de sauter l'étape de découverte d'hôtes.

# Q 2 : Quel est le contrôleur de domaine ? Comment pouvez-vous le déterminer(2 façons distinctes)?

Tous les contrôleurs de domaine écoute le port 389, il suffit donc de scanner ce port afin de savoir qu'elles sont les machines qui l'écoutent.

# Q 3 : A partir de la capture tcpdump, déterminer comment laversion de Windows est récupérée? **SCREENSHOT ° 7**

Les paquets SMB qui effectuent les demandes de sessions contiennent la version de la machine cible. Il suffit donc de filtrer les paquets sur wireshark pour trouver l'OS de chaque machine.

![](/home/jerome/HEIG/Labo/SOS/7.png)

# Q 4 : Quels sont les droits d’exécution que vous obtenez ? **SCREENSHOT ° 37**

Les droits du groupe de NT AUTHORITY\SYSTEM

![](/home/jerome/HEIG/Labo/SOS/37.png)

# Q 5 : Comment expliquer que vous disposez d’autant de privilège ? 

Car on fait partit du groupe NT AUTHORITY\SYSTEM

# Q 6 : Quel processus exécute votre meterpreter sur la machine victime (pid+nom) ? **SCREENSHOT ° 3** 

cmd.exe, pid : 3812

![](/home/jerome/HEIG/Labo/SOS/3.png)

# Q 7 : Quelle est la différence entre la version reverse_tcp et bind_tcp de meterpreter ? 

bind_tcp utilise un port de la machine victime, et donc la manipulation peut être bloquée par un firewall
reverse_tcp tente de connecter la machine victime à la machine de l'agresseur, dès lors c'est la machine attaquante qui doit ouvrir ses ports.

# Q 8 : Dans quelle situation est-il recommandé d’utiliser la version reverse_tcp ? 

Quand la machine victime est protégée par un firewall.

# Q 9 : Dans la sortie de l’exécution, la notion de stage apparait, de quoi s’agit-il ? **SCREENSHOT**

Il s'agit de l'envoi du 2ème payload de notre reverse_tcp

![](/home/jerome/HEIG/Labo/SOS/1.png)

# Q 10: Quels sont les formats de hash utilisés pour stocker les mots de passe dans la SAM ? A quoi correspondent les différentes parties ? **SCREENSHOT ° 4**

Le format utilisé est le NTLM Hash.
NTLM Hash   : username:relative identifier:LM Hash:NT Hash

![](/home/jerome/HEIG/Labo/SOS/4.png)

# Q 11: Comment expliquer que plusieurs comptes partagent les mêmes hashs ? 

Car les utilisateurs ont le même mots de passes.

# Q 12: Quel est le format de hash utilisé pour stocker les hash MS-CACHE ? A quoi correspondent les différentes parties ? **SCREENSHOT ° 4 et 8**

mscash2
Username,Hash,Hash iteration count,Logon Domain Name,DNS Domain Name,Last Login,UPN,Effective Name,Full Name,Logon Script,Profile Path,Home Directory,HomeDir Drive,Primary Group,Additional Groups

![](/home/jerome/HEIG/Labo/SOS/4.png)

![](/home/jerome/HEIG/Labo/SOS/8.png)

# Q 13: À quoi correspond le compte qui se termine par un $ retrouvé dans la mémoire de LSASS ? **SCREENSHOT ° 9**

Le compte créer à partir du nom de la machine lorsque cette dernière à rejoint le domaine

![](/home/jerome/HEIG/Labo/SOS/9.png)

# Q 14: Quel type de compte est nécessaire afin d’accéder au GPO sur le partage SYSVOL ? 

N'importe quel compte standard peut lire les GPO du partage SYSVOL

# Q 15: Quel est l’identifiant de la GPO qui contient le mot de passe ? **SCREENSHOT ° 10**

svc_sched

![](/home/jerome/HEIG/Labo/SOS/10.png)

# Q 16: Quelle est la valeur chiffrée en CPassword qui correspond au mot de passe trouvé dans la GPP ? **SCREENSHOT ° 38**

![](/home/jerome/HEIG/Labo/SOS/38.png)

# Q 17: Est-ce que ce compte (cf. P16) est utilisé sur l’une des machines (smb_login) ? Comment expliquer le résultat que vous obtenez ? **SCREENSHOT ° 12**

Il est utilisé sur le 4 machines car il correspond au "Service scheduler" de Windows

![](/home/jerome/HEIG/Labo/SOS/12.png)

# Q 18: Quel compte avez-vous utilisé pour le module get_user_spn ? Pourquoi ? 

svc_sched, car ...

# Q 19: Illustrer le résultat obtenu et expliquer pourquoi une seule entrée est retournée par le module ? **SCREENSHOT ° 13**

Car il n'y a qu'un seul TGS qui a été récupéré.

![](/home/jerome/HEIG/Labo/SOS/13_1.png)

# Q 20: Quel est le SPN complet vulnérable ? 

MSSQLSvc/WAD-SQL01.WAD.local:1433

# Q 21: Quel est le compte du domaine associé à ce SPN ? 

svc_sql

# Q 22: Est-ce que ce compte est utilisé sur l’une des machines (utiliser smb_login) ? **SCREENSHOT**

Oui, il est utilisé sur toutes les machines que nous avons pu détecter.

![](/home/jerome/HEIG/Labo/SOS/34.png)

# Q 23: Quels sont les privilèges requis pour l’utilisation de psexec ? 

Admnistrator privileges

# Q 24: Quelle vulnérabilité exploitez-vous pour rebondir sur le second serveur ? 

PSexec avec Pass The Hash

# Q 25: Comment avez-vous pu récupérer un compte du domaine sur le second serveur ? **SCREENSHOT ° 19**

Car quelqu'un s'était déjà loggé avec le compte administrator du domaine.

![](/home/jerome/HEIG/Labo/SOS/19.png)

# Q 26: Quelles sont les actions qui justifient l’utilisation d’un compte « Domain Admins » ? 

Installation d'un logiciel, accès à des fichiers critiques, en cas de modification "permanente" de la machine

# Q 27: Comment éviter qu’un de ces comptes puissent être volés ?

Changer régulièrement de mot de passe, avoir des mot de passe forts et n'utiliser les comptes admins du domaine que si cela est strictement nécessaire

# Q 28: Pourquoi migrer dans un processus appartenant à l’utilisateur student3 ? 

Nous migrons un processus pour passer d'une architecture 32 bits à une architecture 64bits  ou pour obtenir plus de privilèges. 

# Q 29: Qu’est-ce qui se passe quand vous essayez de monter le partage la première fois ? Qu’est-ce qui se passe la seconde fois ? Comment expliquer cette différence ? **SCREENSHOT** 

![](/home/jerome/HEIG/Labo/SOS/30_1.png)

L'accès nous est refusé  car nous ne possédons pas les privilèges nécessaires pou faire l'opération.

![](/home/jerome/HEIG/Labo/SOS/35_1.png)

Nous avons fait usage du Golden ticket et donc nous avons maintenant les droits nécessaires pour effectuer cette opération. 

# Q 30: Localiser l’événement d’authentification généré avec le Golden Ticket dans les logs du DC **SCREENSHOT ° 36** 

![](/home/jerome/HEIG/Labo/SOS/36.png)

# Q 31: Combien de temps est valable le golden ticket que vous avez généré ? 

Indéfiniment car le contrôleur de domaine ne garde pas de trace des TGT émis. De ce fait s'il arrive à déchiffrer le TGT, c'est bon pour le contrôleur. 2 

# Q 32: Qu’est ce que l’administrateur du domaine doit faire s’il détecte qu’un attaquant a compromis le hash du compte krbtgt ?

Si l'administrateur venait à se rendre compte de ce problème, il devrait régénérer le **krbtgt** afin d'invalider celui utiliser pour le golden ticket. 