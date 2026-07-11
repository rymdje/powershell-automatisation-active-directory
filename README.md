# Automatisation PowerShell - Active Directory

## Le contexte

Ce projet vient compléter mon premier projet sur Active Directory. Dans le Projet 1, j'avais créé toute une infrastructure Active Directory **à la main** (unités d'organisation, utilisateurs, groupes) pour bien comprendre comment ça marche.

Ici, je fais la même chose mais **automatiquement, avec un script PowerShell**. Au lieu de cliquer pour créer chaque utilisateur un par un, le script crée toute l'infrastructure d'un coup. C'est ce que font les administrateurs en entreprise : quand il faut créer 10, 50 ou 200 comptes, on ne le fait pas à la main, on automatise.

## Ce que j'ai utilisé

* VirtualBox pour la virtualisation
* Windows Server 2022 
* Le domaine `rym.local` 
* PowerShell ISE pour écrire et lancer le script
* Le module ActiveDirectory de PowerShell

Serveur : SRV-AD01

## Ce que fait le script

Le script crée automatiquement une nouvelle entreprise fictive **TechCorp** avec :

* 1 unité d'organisation principale : TechCorp
* 4 services (sous-unités) : Direction, Marketing, Developpement, Support
* 10 utilisateurs répartis automatiquement dans les bons services

Chaque utilisateur est créé avec son prénom, son nom, son identifiant de connexion, son mot de passe et son adresse (UserPrincipalName). La boucle `foreach` parcourt la liste des employés et crée chaque compte au bon endroit.

## Un script qui peut être relancé sans erreur

C'est la partie dont je suis la plus fière. Au début, quand je relançais le script, il plantait en disant que les éléments "existaient déjà". C'est un vrai problème : un compte Active Directory est unique dans tout l'annuaire, donc on ne peut pas le recréer s'il existe encore quelque part.

J'ai donc ajouté un **bloc de nettoyage au début du script** : avant de créer quoi que ce soit, il supprime l'ancienne infrastructure TechCorp et les anciens comptes s'ils existent déjà. Comme ça, le script peut être lancé autant de fois qu'on veut sans jamais planter. En informatique on appelle ça un script **idempotent** : peu importe combien de fois on le lance, on obtient toujours le même résultat propre.

## Les étapes

1. **Écriture du script** dans PowerShell ISE, en administrateur, sur le serveur SRV-AD01.
2. **Nettoyage** : le script supprime d'abord toute ancienne trace de TechCorp (unités et comptes).
3. **Création des unités d'organisation** : TechCorp et ses 4 services.
4. **Définition du mot de passe** commun (uniquement pour ce labo).
5. **Liste des employés** : un tableau avec les 10 personnes et leur service.
6. **Boucle de création** : le script parcourt la liste et crée chaque utilisateur dans son service.
7. **Vérification** dans la console Active Directory que tout est bien créé.

## Captures

Les captures sont dans le dossier `captures` : le script complet, le résultat de l'exécution (les 10 utilisateurs créés), et la console Active Directory qui montre l'entreprise TechCorp remplie avec ses services et ses utilisateurs.

## Ce que ça m'a appris

* écrire un script PowerShell pour administrer Active Directory
* utiliser une boucle `foreach` pour automatiser une tâche répétitive
* créer des utilisateurs et des unités d'organisation par script (`New-ADUser`, `New-ADOrganizationalUnit`)
* comprendre qu'un compte Active Directory est unique dans l'annuaire
* rendre un script relançable sans erreur (idempotent) grâce à un bloc de nettoyage
* lire et comprendre les messages d'erreur pour corriger un script étape par étape


