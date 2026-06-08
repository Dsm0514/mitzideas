---
title: "Tout a un début"
slug: "tout-a-un-debut"
date: 2026-05-14T10:00:00-03:00
draft: false
categories: ["Début"]
series: []
description: "Le début de MitzIdeas."
translationKey: "primeiro-post"
---

Bonjour ! Je m'appelle Davi Schmitz, j'ai 21 ans (j'ai intentionnellement publié ce premier post le jour de mon anniversaire), je suis un développeur Junior qui travaille actuellement avec le framework Django et je suis étudiant en informatique.

<small>J'ai jugé nécessaire de me présenter ici car je mettrai à jour la page « À propos » au fil du temps, cette introduction sera donc probablement obsolète bientôt.</small>

---

**MitzIdeas** est mon blog — mon espace pour enregistrer des idées, des études et des expériences.

Ce blog est un registre de ma vie : carrière, études, loisirs, livres que je lis et tout ce qui me semble intéressant de partager.

L'objectif principal est que, grâce à ce projet, je puisse consolider les connaissances que j'acquiers lors de mes études, car on apprend beaucoup en essayant d'enseigner ou d'expliquer à quelqu'un d'autre. Je souhaite également développer mon écriture, chose que je veux faire depuis longtemps. De plus, comme je ne vois pas ce projet à court terme, ce que j'écris ici servira d'historique pour que mon « moi futur » se souvienne et comprenne tout le parcours effectué. Mes erreurs, mes réussites, mes idées... tout sera sauvegardé.

Concernant mon écriture, je sais que j'écris encore assez mal. Souvent, je change de sujet et je finis par revenir à mon idée précédente. Cette caractéristique est presque ma signature. J'utiliserai probablement des IA pour m'aider à corriger mes fautes de grammaire et suggérer des améliorations, mais je garantis une chose : je ne permettrai *JAMAIS* aux IA d'avoir le dernier mot sur le résultat final de mon texte. Avant tout, je veux mettre mon identité dans chaque publication. Internet est déjà plein de textes superficiels générés par l'IA, ce que je ne supporte plus.


---

C'est la première publication de mon blog et je ne pouvais pas manquer l'occasion de raconter un peu comment il a été créé. Si vous travaillez dans le domaine de la technologie, vous avez probablement reconnu le look de ce blog quelque part. Je suis Fabio Akita depuis un moment parce que son contenu est essentiel pour quiconque veut vraiment apprendre la programmation, et j'ai utilisé pratiquement les mêmes outils que lui pour créer mon propre blog. Pour dire la vérité, je n'avais aucune idée de comment utiliser ces outils et j'apprends encore, mais comme nous sommes à une époque où nous pouvons produire des choses géniales avec l'aide des LLM, Claude AI a été d'une grande aide pour donner au blog l'apparence que je souhaitais.

Me connaissant, il est fort probable que j'apporte encore plus de modifications à l'avenir, j'ai donc décidé de garder ici une capture d'écran de la première version de la page d'accueil du blog.

{{< figure src="MitzIdeasHome.png" title="La première version de la page d'accueil de MitzIdeas (Mai 2026)." alt="Capture d'écran de la page d'accueil du blog" >}}

Cependant, ce qui est le plus intéressant ici, c'est ce qu'il y a derrière la structure, ce qui nous permet de créer des posts comme nous le souhaitons, rapidement, tout en restant organisés. À cette fin, je vais essayer de donner une brève introduction sur RSS, Hextra et Hugo, qui est le moteur derrière tout cela. J'expliquerai également la traduction ou l'internationalisation du blog, plus précisément comment je traduis la structure et le contenu des publications.

### Comprendre le RSS

RSS signifie *Really Simple Syndication*. De manière simplifiée, il s'agit d'un fichier XML qui répertorie vos publications récentes, permettant aux lecteurs de flux comme Feedly ou NetNewsWire de savoir que vous avez publié quelque chose sans qu'ils aient besoin de visiter votre site.

### Hugo

Hugo est un générateur de sites statiques (SSG - Static Site Generator) écrit en Go. Chaque partie de ce blog est un fichier Markdown (.md) que Hugo traite via les modèles HTML que j'ai dans un dossier appelé *layouts* et génère un site statique (HTML/CSS/JS pur) dans le dossier *public*. Il est extrêmement rapide (génère des centaines de pages en millisecondes) et sécurisé, car il n'a pas de base de données en temps réel sur le serveur. Comme tout est statique, je peux l'héberger gratuitement sur diverses plateformes, ce qui est exactement ce que je recherchais : pouvoir partager mes idées sans avoir à payer d'hébergement.

### Hextra

Hextra est le thème ou framework de design que j'utilise par-dessus Hugo. Il facilite beaucoup de choses, comme la création de la barre de recherche des publications, la configuration du *Dark Mode* et du *Light Mode* ainsi que l'organisation des sections. Je l'ai personnalisé pour avoir un aspect plus « clean » et minimaliste, car l'intention était que le contenu reçoive presque toute l'attention du lecteur ; j'ai toujours eu des problèmes avec les sites ou interfaces qui affichent trop d'informations à l'écran.

### Internationalisation et traduction

Vous avez probablement remarqué que le site et les articles sont traduits dans d'autres langues. Pour cela, j'ai utilisé le *Multilingual Mode* d'Hugo. Vous vous souvenez que j'ai mentionné que chaque partie du blog est un fichier Markdown ? Je sauvegarde chacun d'eux avec les informations de langue dans l'extension, comme *.en.md* pour l'anglais. Exemples : *primeiro-post.en.md* pour l'anglais ou *primeiro-post.fr.md* pour le français. Hugo crée des arborescences de répertoires séparées pour chaque langue dans la construction finale (/en/, /es/, etc.). De plus, j'utilise des fichiers dans le dossier *i18n/* ou des paramètres dans *hugo.toml* pour traduire des boutons comme « Retour » ou « Suivant ».

---
Mettre en place toute cette structure à partir de zéro a déjà été un grand défi, je vais donc terminer ce premier article ici pour le garder plus simple. Au fur et à mesure que j'acquerrai de l'expérience et de la fluidité avec ces outils, les prochains textes seront de plus en plus élaborés.