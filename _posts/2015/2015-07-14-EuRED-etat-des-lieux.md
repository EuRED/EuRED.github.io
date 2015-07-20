---
title: 'EuRED, état des lieux'
date: 2015-07-15T05:12:40.000Z
last_modified_at: 2015-07-15T05:43:57.000Z
featured_image: /uploads/2015/20150715--keyboard_w1200.jpg
---

# EuRED, état des lieux
En ce jour de défilé au rythme d'une marche, j'ai moi-même avancé sur mes développements pour le projet RECIHC.

Ce projet universitaire européen vise notamment à préparer une future base de données des expériences de lecture, du XVIIe S. à nos jours. Une expérience de lecture ? Il s'agit de recenser dans cette base de données le témoignage direct ou indirect d'une situation où quelqu'un lit, seul ou en groupe, et d'en tirer des informations à des fins d'études. Quel est le texte lu ? A qui ? Dans quel contexte ? Est-ce qu'un lecteur ou un auditeur fait part de ses émotions ?

Le point de départ pour cette partie du projet RECIHC est la base de données UK Red (Reading Experience Database), qui a maintenant près de 10 ans. Je suis développeur et dirigeant d'idéesculture, une petite société basée sur le campus du Mans, et intervient en tant qu'ingénieur sur le projet RECIHC. Pour ce projet, comme dans ma société, je travaille principalement autour du projet libre de gestion et publication des collections CollectiveAccess. Ce projet de base EuRED vise à utiliser CollectiveAccess comme outil de gestion et de publication de la base de données.

J'ai donc préparé, avec l'aval de Brigitte Ouvry-Vial, responsable du projet, et directement avec François Vignale, conservateur à la BU du Mans, différents essais autour des données d'UK Red. Contrairement à UK Red, nous avons tout de suite orienté notre réflexion sur le souhait de se baser sur un standard existant, TEI, éventuellement en l'amendant au besoin. TEI (Text Encoding Initiative) permet de retranscrire un document, même manuscript, en XML et d'y apporter des notes par le biais d'une large sélection de balises.

Nous sommes partis d'une idée simple mais puissante, considérer qu'il nous manquait une balise simple pour indiquer un témoignage d'une expérience de lecture. Le reste se situe dans le fait de rajouter une surcouche d'information dans cette balise, par des attributs et des renvois vers d'autres parties du fichier XML.

## Premières implémentations
![]() Grille de saisie dans CollectiveAccess. Ici tous les conteneurs rassemblant les champs sont réduits pour permettre une vue globale.

Dans CollectiveAccess, une fiche d'expérience de lecture reprend les données d'une fiche sur le principe d'UKRed.

![]() Capture d'écran du projet _Lettres et textes, le Berlin intellectuel des années 1800_

J'ai modifié assez fortement le thème par défaut de l'interface publique de CollectiveAccess pour intégrer une visionneuse complète, en suivant les concepts mis en oeuvre dans le projet "LETTRES ET TEXTES: LE BERLIN INTELLECTUEL DES ANNÉES 1800" qui met en ligne des lettres manuscriptes du XIXe S. en TEI.

![]() Implémentation dans la partie publique de CollectiveAccess

A ce stade, l'expérience de lecture est recensée sous la forme :
- d'une fiche décrivant l'expérience
- d'autres fiches liées pour décrire la source qui contient le témoignage de l'expérience (c'est à dire par exemple une lettre qui signale que son auteur a lu tel ouvrage), et pour décrire l'ouvrage qui est lu
- d'un média rattaché à l'expérience si on possède un scan du document source pour la portion qui concerne le témoignage
- d'un fichier XML TEI stocké dans un champ de type fichier directement dans la fiche de l'expérience : celui-ci contient une retranscription du témoignage augmenté de la balise \<readingExp\>.

## Limites et interrogation
Notre implémentation est assez "sexy" visuellement, on a 6 interface sur le même principe que sur la base des intellectuels allemands, grâce à CollectiveAccess, on a une base de données bien structurée, facilement enrichissable, bref de quoi démarrer.

Pourquoi s'interroger à ce stade ?

Nous avons à ce niveau un outil permettant de mettre en place une base similaire à UKRed avec, **en plus**, la possibilité de gérer le XML TEI.

Où est le problème ?

En fait, il n'y en a pas pour gérer une suite d'expériences de lecture, mais je perçois les limitations dès maintenant :
- nous avons une visionneuse pour l'interface publique, mais rien de particulier côté gestion dans CollectiveAccess
- si nous avons plusieurs expériences de lecture au sein d'un même document, nous allons saisir plusieurs fiches, et donc faire des doublons pour les documents rattachés : XML et scan le cas échéant

## Conceptualiser
Si considérer qu'une collection de fiches d'expériences de lecture n'est pas satisfaisante, il faut renverser le paradigme.

Que doit contenir la base ?

Si la source de l'expérience de lecture est unique, alors que les témoignages, récits d'autant d'expériences de lecture, peuvent être multiples à l'intérieur, ceux-ci doivent être au centre de la base.

Dès lors, il faut pouvoir relier plusieurs expériences à une même source. Si le lien est direct, nous retournons la situation actuelle : au lieu de se centrer sur l'expérience, on centre la réflexion sur la source à laquelle on raccroche nos témoignages.

Peut-on trouver mieux ? Peut-être.

Dans le cadre du workshop RECIHC fin mai à l'Université du Maine, l'objectif de pouvoir décrire d'autres types de sources que des textes était clairement rappelé par les participants. Fonctionnellement, la structure envisagée jusqu'ici le permettait, je n'ai donc vu aucun blocage.

Aujourd'hui, au lieu d'y voir une absence de blocage, j'y vois une fantastique opportunité.

CollectiveAccess dispose d'une excellente visionneuse d'image, capable d'annoter les médias rattachés.

![]() La visionneuse de CollectiveAccess avec une annotation centrée à l'écran et la liste des annotations rattachées à l'image

Fonctionnellement, l'ajout de balise readingExp au sein d'un document TEI se rapproche de l'ajout d'annotation à une image.

![]() Saisie des métadonnées d'une annotation dans CollectiveAccess

Au delà d'un simple titre, on peut ajouter des métadonnées à une annotation dans CollectiveAccess.

Dans CollectiveAccess, un média est un fichier attaché à une fiche de la base.

L'outil d'annotation de CollectiveAccess permet d'annoter une partie d'un média : avec des coordonnées spatiales à deux dimensions, pour permettre de rattacher un point, un rectangle ou un tracé libre dans l'image. De la même manière, on peut annoter une séquence au sein d'une vidéo dans CollectiveAccess (avec un timecode de début et un timecode de fin).

Nous imaginons donc d'énormes perspectives d'avenir pour un système stratifié :
- un document source, décrivant le document qui contient les témoignages
- un ou des médias rattachés, les scans des documents sources, une ou plusieurs pages, vidéos, images, etc
- un média TEI XML rattaché au document source
- une ou des annotations rattachées au média : nos expériences de lecture

Une expérience de lecture serait alors une annotation dans le XML TEI décrivant ou retranscrivant le document source :
1. métadonnées de description de la source
2. fichier XML TEI
3. readingExp dans le XML TEI, simplement défini par des coordonnées simples : le point d'insertion correspondant au début du témoignage et le point correspondant à la fin du témoignage

Ce système permet donc que les témoignages se chevauchent. L'enrichissement du fichier XML TEI est alors possible via les métadonnées des annotations readingExp, qui contiennent eux-même autant de liens que nécessaire vers d'autres enregistrements de la base de données : lieux, personnes, événements, vocabulaires et ontologies...

## Pistes de développement
Il nous faudra donc préparer CollectiveAccess dans les mois à venir pour gérer des médias de type XML TEI, et développer un support complet des annotations au sein du XML pour gérer nos readingExp. Dans un 2e temps, il faut également que le XML TEI soit mis à jour soit en temps réel, soit régulièrement pour qu'il s'enrichisse des annotations saisies dans l'interface.

## Ouverture
Ce système permet d'envisager une solution technique participative, avec une construction partiellement automatisée.

### Automatisation de l'enrichissement
On peut envisager de détecter automatiquement via des recherches externes des documents déjà décrits en XML TEI, et permettre leur import dans la structure CollectiveAccess à un niveau simple : création d'une source, rapatriement du XML TEI et rattachement en tant que média, création d'annotation a minima à partir d'un mot-clé ou d'une expression clé identifié(e), passage dans un statut "à valider" pour permettre un enrichissement manuel ultérieur.

### Participation
L'enrichissement de la base doit pouvoir être sur base volontaire, comme sur UK Red. Avec cette idée de rebâtir notre future base avec pour pivot les annotations, comme outil de description des expériences de lecture, on peut imaginer partager dans l'interface publique cet outil d'annotation. Le modèle que nous avons en tête est la saisie collaborative, comme dans certaines archives départementales pour la retranscription des registres d'état civil. Imaginons un instant. Une simple invite, bien visible à l'écran, sur le portail public, invite le visiteur à donner 1 minute, 3 minutes ou 5 minutes de son temps, pour retranscrire un des témoignage de lecture pré-sélectionné. Dans l'idée, une fiche de saisie, à l'image du formulaire public de saisie sur UK Red, mais prérempli. Cette saisie d'une petite main bénévole sur internet rejoindrait la base, dans un statut à valider ou tout autre.

# Conclusion
Les effets de bord de notre idée seraient nombreux : amélioration exponentielle de l'usage possible de CollectiveAccess pour gérer une base documentaire en TEI, outils d'annotation public, interface de saisie... Et nous nous renonçons à rien : on peut imaginer une saisie d'une expérience de lecture à partir de zéro, lorsque nous n'avons pas de fichier TEI à soumettre : ce sera le cas des documents issus de UK Red, dont le contenu ira enrichir cette future base EuRED. Un plugin basique dans l'interface de gestion ou dans celle publique de CollectiveAccess créera 3 choses en automatique : création d'une fiche source, rattachement d'un XML "vierge" calibré en fonction de nos pratiques pour TEI, et ouverture directement d'une interface de saisie pour la description d'une expérience de lecture. Si nous avons la retranscription du document source, voir également d'un scan, là, nous ouvrons la porte vers une toute nouvelle expérience.
