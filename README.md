# aaresumedeladecision
This repository is the French version of aadecisionoverview

# Instructions pour les diapositives

Ce projet utilise mkdocs, Kobo, et un peu de JavaScript pour créer un parcours éducatif ou un flux de décision dont les résultats sont enregistrés dans Kobo. Il utilise un système simplifié de « Configuration Div » pour gérer la logique des diapositives (formulaires, redirections, images et persistance de l’email). Au lieu d’écrire du HTML/JS complexe dans chaque diapositive, il suffit d’ajouter une ligne de configuration au bas de chaque fichier whatever.md dans docs.

- L’email saisi dans le formulaire est conservé pour les pages suivantes et apparaît automatiquement dans les formulaires Kobo si tout est configuré correctement. L’utilisateur n’a qu’à soumettre les formulaires ou cliquer sur le bouton « aller à la page suivante » en bas de l’écran. Le JavaScript s’occupe du reste.
- Si l’utilisateur navigue via le menu latéral, la persistance de l’email peut ne pas fonctionner.
- Vous pouvez voir l’email dans l’URL sous la forme : /?email=a@b.c.

En théorie, vous pouvez créer un nouveau dépôt GitHub mkdocs, copier ces fichiers, puis les adapter à vos besoins.

- Vous aurez également besoin d’un compte Kobo.

- Certains dépôts GitHub contiennent aussi des figures générées automatiquement selon les réponses des utilisateurs, mais cette documentation ne couvre pas cet aspect.

- Documentation mkdocs utilisée :
https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site

-Commandes mkdocs les plus utilisées (à exécuter à la racine du projet) :
 	- Pousser les modifications et publier le site en externe: '''mkdocs gh-deploy'''
	- Voir le site en temps réel : '''mkdocs serve'''

## Contenu du dépôt

- Le dossier docs contient le texte visible par les utilisateurs, chaque page étant un fichier nommé nom.md, ainsi que les fichiers image auxquels ces pages font appel.
	- À l’intérieur du dossier docs se trouvent les dossiers stylesheets et javascripts, qui contiennent le 	code gérant la navigation et la mémorisation des emails.
	- Le dossier javascripts pourrait être déplacé dans un dépôt séparé à l’avenir pour améliorer la 		cohérence, mais cela rendrait l’ensemble plus complexe.
- Le dossier koboforms contient les formulaires Kobo utilisés et téléversés(upload) dans Kobo pour le workflow.
- Le dossier site est généré automatiquement, vous n’avez donc pas besoin de vous en occuper.
- Le fichier mkdocs.yml contient :
	- Le flux de navigation affiché sur le côté gauche de l’écran. Vous devez y ajouter chaque fichier 	sous la forme - 'filename.md' pour que chaque page fonctionne correctement.
	- Les paramètres liés au formatage et l’emplacement du JavaScript.

## Comment utiliser

- Vous créez un fichier Markdown nom.md pour chaque page de votre workflow.
- La première page doit s’appeler index.md.
- Assurez-vous de modifier le fichier mkdocs.yml pour y inclure chaque page du workflow.
	- Cela se fait sous la ligne nav: et ressemble à : - 'afilename.md'
	- Si vous ne le faites pas, la plupart des éléments fonctionneront, mais la barre de navigation 		latérale sera cassée.
- Au bas de chaque fichier Markdown, vous devez envoyer une ligne d’information au JavaScript pour lui indiquer ce que vous voulez qu’il fasse. Cette ligne peut être écrite sur une seule ligne ou répartie sur plusieurs lignes si vous préférez.
	- Exemple tiré de index.md, qui sert simplement à récupérer l’email des utilisateurs pour les 		diapositives suivantes :
	'''<div id="slide-config" data-type="start" data-next="../disastermandate/"></div>'''
- **Important : vous devez indiquer dans cette ligne le nom de la diapositive suivante.**
	- Cela signifie que vous devez définir l’ordre des diapositives à la fois dans mkdocs.yml et dans 	chaque '<div>' au bas des fichiers .md.

### Diapositive de démarrage (Saisie de l’email)

Point d’entrée du workflow. Cette diapositive demande à l’utilisateur son email, l’enregistre dans le session storage, puis le redirige vers la diapositive suivante.

Exemple de fichier md dans ce dépôt : index.md

Attributs :

data-type="start"

data-next : chemin vers la première vraie diapositive de contenu.

Exemple :
'''
# Début 
Commençons !

<div id="slide-config" 
     data-type="start" 
     data-next="../slide1/">
</div>
'''

### Diapositive Formulaire Kobo

Affiche un formulaire KoboToolbox. L’email de l’utilisateur est automatiquement prérempli (s’il a été capturé auparavant) et la redirection après soumission est gérée automatiquement.

Quelques notes sur Kobo (une documentation plus détaillée serait utile) :
	- La meilleure façon de créer un formulaire Kobo est de copier l’un des fichiers .xlsx du dossier 	koboforms.
		- Ensuite, aller dans Kobo → Forms, cliquer sur New, puis Upload XLSX form, et glisser votre 		fichier.
		- Secteur : "Education", pays : "États-Unis".
		- **Assurez-vous que l’option “Allow submissions to this form without a username and 			password” (Autoriser les soumissions à ce formulaire sans nom d'utilisateur ni mot de passe) 		est activée.
		- Cliquez sur "DEPLOY" (DÉPLOYER).
		- Cliquez sur le bouton "open" (Ouvrir) en bas. L’URL affichée sera du type :
		https://ee.kobotoolbox.org/x/YourID  
		→ La partie YourID est celle à utiliser dans data-kobo-id="YourID".

Exemple de fichier dans ce dépôt : game_time.md

Attributs :

data-type="kobo"

data-next : Chemin vers la diapositive suivante (ex. ../slide2/ ou slide2.html).

data-kobo-url : L’URL de votre formulaire Kobo (ex. https://ee.kobotoolbox.org/x/YourID).

Exemple :

'''# Questions

Remplissez ce formulaire

<div id="slide-config" 
     data-type="kobo" 
     data-next="../slide3/" 
     data-kobo-id="b5CVmfVW">
</div>
'''
### Diapositive Figure

Affiche une image ainsi qu’un bouton "Next" (Suivant). Un horodatage est automatiquement ajouté à l’URL de l’image afin de forcer un rechargement et d’éviter les problèmes de cache.

Exemple de fichier dans ce dépôt : peoplesinvestments.md

Attributs :

data-type="figure"

data-next : Chemin vers la diapositive suivante.

data-img : Chemin vers votre fichier image (ex. assets/chart.png, ou même une URL).

Exemple :
'''# Results

Voici ce que les gens ont indiqué dans les formulaires :

<div id="slide-config" 
     data-type="figure" 
     data-next="../slide4/" 
     data-img="https://dosgoodcu.github.io/auto-assets/public_investment_chart.png">
</div>
'''
Diapositive Texte Simple

Affiche du texte Markdown standard avec un bouton "Next" (Suivant) en bas de la page.

Exemple dans ce dépôt : savingspoints.md

Attributs :

data-type="simple"

data-next : Chemin vers la diapositive suivante.

Exemple :
'''# Information
Lisez ce texte important. Cliquez sur le bouton ci‑dessous lorsque vous avez terminé.

<div id="slide-config" 
     data-type="simple" 
     data-next="../slide2/">
</div>
'''

### Diapositive Finale

Insérez uniquement le Markdown que vous souhaitez afficher, sans aucun code.

Exemple dans ce dépôt : bye.md

Exemple :
'''# Bye

Merci !
'''

### Markdown utile

Autres éléments Markdown utiles qui fonctionnent dans cet environnement :

- Liens :
	- Le plus simple : écrire directement le lien en texte
	'''http://https://dosgoodcu.github.io/whatsthepointgameinteractive'''
	- Standard (fonctionne aussi pour les figures locales) :
	'''[Voir le tableau de bord démo du Niger]
	(https://iridl.ldeo.columbia.edu/fbfmaproom2/niger_demo)'''
	- Lien ouvrant dans un nouvel onglet avec MkDocs :
	'''
	<a href="https://iridl.ldeo.columbia.edu/fbfmaproom2/niger_demo" target="_blank">
     		Voir le tableau de bord démo du Niger
	</a>
	'''
- Figure simple:
'''![](hat_gum.png)'''

- Figure plus élaborée (si vous devez contrôler la taille)
	'''<img src="hat.jpg" alt="Hat" style="width:500px;">'''

- Fenêtre URL intégrée (par exemple un outil cartographique interactif):
	'''
	<div style="text-align: center; margin-top: 10px;">
    		<iframe id="resizableFrame"
        			src="https://fist.iri.columbia.edu/publications/docs/Madagascar_AA_FLexDashboard_OND_2024_FR/"
        			width="1200" height="1300"
        			style="border:1px solid black; transition: all 0.3s ease;"></iframe>
</div>
'''

## Comment créer un dépôt mkdocs similaire à partir de celui‑ci

Voici les instructions mises à jour.

Ces étapes supposent que le **code source** se trouve dans 'ccsfist/whatsthepointfi' et que vous (connecté en tant que **ccsfist**) souhaitez créer un nouveau dépôt indépendant (par exemple 'newrepo') basé sur ce code.

### Cloner et renommer (Terminal local)

Téléchargez le code "modèle" et renommez le dossier selon le nom de votre nouveau projet.

1. **Cloner le modèle ccsfist :**
'''bash
git clone https://github.com/ccsfist/aaresumedeladecision.git

'''


2. **Renommer le dossier : **

Remplacez 'newrepo' par le nom que vous souhaitez donner à votre projet.
'''bash
mv aaresumedeladecision newrepo

'''


3. **Entrer dans le nouveau dossier :**
'''bash
cd newrepo

'''



### Couper le lien (le “Nouveau départ”)

Vous devez supprimer l’historique Git existant afin que ce nouveau projet n’essaie pas de se synchroniser avec 'aaresumedeladecision'.

1. **Supprimer l’ancien historique :**
'''bash
rm -rf .git

'''


2. **Créer un nouveau dépôt :**
'''bash
git init
git branch -M main

'''


### Mettre à jour l’identité

Vous devez changer le **Nom** afin que l’onglet du navigateur n’affiche plus "Cogeneration‑Satellite…".

**Ouvrez 'mkdocs.yml.'**

**Modifiez 'site_name' (nom du site) tout en haut :
'''yaml
site_name: New Repo  <-- Remplacez ceci par votre nouveau titre

'''
**Enregistrez le fichier.**

### Téléverser et publier

De retour dans votre terminal (à l’intérieur du dossier 'newrepo') :

**Valider les fichiers :**
'''bash
git add .
git commit -m "Commit initial : création du site MkDocs"

'''

**Créer un nouveau dépôt :**
Vous devrez peut‑être vous connecter. 

'''
gh repo create ccsfist/newrepo --public --source=. --remote=origin --push
'''


**Connecter votre dépôt local au nouveau dépôt GitHub :**

Si vous avez oublié de changer le nom du dépôt (par exemple si newrepo.git n’est pas correct), vous pouvez corriger l’URL avec cette commande :
'''
git remote set-url origin https://github.com/ccsfist/newrepo.git
'''

**Pousser le code :**
'''bash
git push -u origin main

'''

**Publier le site :**
*(Cette commande génère le HTML et le pousse dans la branche gh-pages.)*
'''bash
mkdocs gh-deploy

'''



**Terminé !**

* **Code :** https://github.com/ccsfist/newrepo
* **Site :** https://ccsfist.github.io/newrepo (Attendez environ 2 minutes pour qu’il apparaisse.)
