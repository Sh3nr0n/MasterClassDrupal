# Etapes Master Classe Drupal
 - Installer drupal avec composer
 - Prérequis : avoir une version de composer supérieure à 1.0 et CURL doit être activé dans le fichier "php.ini"
 - Se placer dans le répertoire ou créer le projet 'my_site_name_dir'
 
       composer create-project drupal-composer/drupal-project:8.x-dev my_site_name_dir

 - Rechercher des modules
https://www.drupal.org/project/project_module
 - L'installation se fait via composer avec require drupal /nom_du_module

 - Configuration du template via le fichier yaml
 - Fichiers twig pour introduire les templates dans le html

 - Dans Web/themes : créer un dossier /custom qui va recevoir les templates personnalisés et un dossier /contrib qui va recevoir les templates de la communauté drupal
 - Coller le dossier /template dedans
 - Déplacer le dossier dist dans le dossier parent
 - Créer un dossier /templates dans le dossier /template (sans "s") dans lequel il faut créer les fichiers twig suivants


       /mytheme  |
                       ---mytheme.libraries.yml
                       |
                       |
                       ---mytheme.info.yml
                       |
                       |
                       ---mytheme.theme ->Preprocess
                       |
                       |
                       ---/css -> votre css (on peut aussi        laisser ce dossier dans /dist)
                       |
                       |
                       --- /js -> votre js (on peut aussi        laisser ce dossier dans /dist)
                       |
                       |
                       ---/templates
                       |
                       |
                       ---logo.jpg

 - Ces fichiers peuvent être récupérés depuis le dossier /web/core/themes/stable/templates après l'installation de base de drupal.
 - Inclure un fichier page.html.twig dans ce sous dossier
 - Renommer le fichier index.html en index.html.twig

 - Paramétrer les fichiers mytheme.info.yml et mytheme.libraries.yml

 - Fichier librairies.yml :

       version: VERSION de l'app
       css:
         base:
           dist/bulma/bulma.min.css: {} => chemin vers le      css du template et des librairies utilisées
           dist/swiper/css/swiper.min.css: {}
           dist/css/global.css: {}
       js:
         dist/swiper/js/swiper.min.js: {} => chemin vers      les fichiers js utilisés
         dist/js/app.js: {}

 - Fichier info.yml




    
       name: mytheme => le nom du thème
       type: theme => le type de theme
       description: ajouter ici une description si vous    le mettez à dispo de la communauté
       core: 8.x => La versiuon principale de Drupal    utilisée
       screenshot: "" => Possibilité d'ajouter une image
   
       regions: => Les différentes sections de nos pages
         header: Header
         content: Content
           footer: Footer
       librairies:
         mytheme/global => nom du thème + / + nom de la librairie tel que défini dans libraries.yml


 - Copier le contenu ci-dessous dans page.html.yml

       {# 
       /**
        * @file
        * Showcase Lite's theme implementation to display a        single page.
        *
        * The doctype, html, head and body tags are not in        this template. Instead they
        * can be found in the html.html.twig template        normally located in the
        * core/modules/system directory.
        *
        * Available variables:
        *
        * General utility variables:
        * - base_path: The base URL path of the Drupal        installation. Will usually be
        *   "/" unless you have installed Drupal in a        sub-directory.
        * - is_front: A flag indicating if the current page        is the front page.
        * - logged_in: A flag indicating if the user is        registered and signed in.
        * - is_admin: A flag indicating if the user has        permission to access
        *   administration pages.
        *
        * Regions:
        *- page.textleft:
        * - page.form:
        * @see template_preprocess_page()
        * @see html.html.twig
        */ #}
        
       <header role="banner">
           {{ page.header }}
       </header>
       {{ page.content }}
       <footer role="contentinfo">
           {{ page.footer }}
       </footer>

 - Créer une base de données vide (avec postgreSQL par exemple)

 - Se placer dans le dossier web et entrer la commande suivante

        php -S localhost:8000

 - Aller dans le navigateur et entrer l'adresse suivante:

        localhost:8000/core/install.php
 - Choisir un langage et cliquer sur "Save and continue"
 - Profil Standard
 - Pour l'hôte entrer 127.0.0.1 (localhost peut entraîner un bug) + entrer les infos de connexion à la base de données
 - L'installation automatique de drupal se lance : renseigner les informations requises

 - Plugins recommandé pour vider le cache Drupal: drush
 - Se placer dans le dossier du projet

       composer require drush/drush

 - Autres commandes pour installer des extensions en global (exemple ici avec drush)
 - Faire un cd pour retourner dans le home

       composer  global require drush/drush:8
 - Récupérer le repo github de drush

       wget https://github.com/drush-ops/drush/releases/download/8.1.16/drush.phar
 - Attribuer les droits d'executer des commandes à drush (à confirmer)

       chmod +x drush.phar
 - Déplacer drush dans le dossier bin de votre machine

       sudo mv drush.phar /usr/local/bin/drush

 - Installer les modules (= extensions) admin tool, paragraph et devel, (à tester : "chaos tool suite")

       composer require drupal/paragraphs
       composer require drupal/devel
       composer require drupal/admin_toolbar
       composer require drupal/entity_reference_revisions

 - Se rendre dans l'interface admin en localhost:
 http://localhost:8000/admin/modules
 - Aller dans COEUR et cocher les cases pour le modules à activer:
 
    - Admin => tout cocher
    - Devel => Cocher les 3 premiers
    - Paragraphs => tout cocher
    - Entity Reference Revisions => cocher 

 - Cliquer sur "Installer"

 -  Commande pour lancer un "rebuild cache" si **drush cr** *ne fonctionne pas*

        drush cache-clear drush
  

 -  Aller dans le menu structure > type de contenu > Ajouter un type de contenu
 -  Entrer le nom et aller décocher toutes les cases dans "Options de publication", "Paramètre d'affichage" et , Paramètre du menu" puis enregistrer les modifications
 - Déplacer le dossier du projet (/app) dans le var/www/html de la machine
 - Dans le navigateur, se rendre à l'adresse du projet: (ex : http://localhost/drupal/app/web/) et cliquer sur **se connecter**
 - Se rendre dans :
        
       /etc/apache2/site-enabled/000-default.conf

 - Remplacer le contenu du fichier par celui présent à la racine de ce dépot dans la même arborescence

 - Entrer la commande suivante pour activer la réécriture des URL par PHP

        sudo a2enmod rewrite
        sudo service apache2 restart
        
**On peut à présent se connecter au site Drupal au prix de lourds efforts de configuration : Happy hacking!**

 - Aller dans le menu Configuration > Performance (rubrique Développement) et décocher "Agréger les fichiers CSS" et "Agréger les fichiers JavaScript" pour rétablir un affichage correct. N'oubliez pas de vider le cache avec drush *cache-clear drush* ou *drush cr*


