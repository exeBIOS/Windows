📌 💻 Étape 1 : Vérifier le bon fonctionnement des sites web sur les serveurs autonomes
🔹 Sur LON-SVR1
Ouvre l'Explorateur de fichiers.

Va dans C:\inetpub\wwwroot.

Double-clique sur iisstart.png pour l'ouvrir avec Paint.

Dessine un cercle autour du logo IIS.

Enregistre (CTRL + S) et ferme Paint.

Ferme l'Explorateur de fichiers.

🔹 Sur LON-DC1
Ouvre Internet Explorer.

Dans la barre d'adresse, tape : http://LON-SVR1 et appuie sur Entrée.

Vérifie que la page IIS affiche le logo modifié avec le cercle.

Ensuite, tape http://LON-SVR2 et appuie sur Entrée.

Vérifie que la page IIS n'a pas le cercle (serveur différent).

📌 💻 Étape 2 : Installation de l'équilibrage de charge réseau (NLB)
🔹 Sur LON-SVR1 et LON-SVR2
Ouvre Gestionnaire de serveur.

Clique sur Gérer → Ajouter des rôles et fonctionnalités.

Dans l'assistant, clique sur Suivant jusqu'à l'étape Rôles de serveurs.

Coche Équilibrage de la charge réseau (NLB).

Clique sur Suivant → Installer et attends la fin de l'installation.

Redémarre les serveurs si nécessaire.

📌 💻 Étape 3 : Création du cluster NLB
🔹 Sur LON-SVR1
Ouvre Gestionnaire de serveur.

Va dans Outils → Gestionnaire de l'équilibrage de la charge réseau.

Dans la fenêtre, clique sur Cluster → Nouveau.

Dans la case Nom de l'hôte, tape LON-SVR1 et clique sur Se connecter.

Sélectionne l'interface réseau (Ethernet ou autre, selon Get-NetAdapter).

Clique sur Suivant.

Dans Adresses IP du cluster, clique sur Ajouter.

Adresse IP : 192.168.161.250

Masque de sous-réseau : 255.255.255.0

Clique sur OK, puis Suivant.

Dans Paramètres du cluster :

Nom complet d'Internet : LON-NLB.

Mode d'opération : Multidiffusion.

Clique sur Suivant.

Dans Règles de port, laisse tout par défaut et clique sur Terminer.

📌 💻 Étape 4 : Ajouter un deuxième hôte au cluster
🔹 Sur LON-SVR1
Dans Gestionnaire de l'équilibrage de la charge réseau, clique sur LON-NLB (192.168.161.250).

Clique sur Cluster → Ajouter un hôte au cluster.

Dans Nom de l'hôte, entre LON-SVR2 et clique sur Se connecter.

Sélectionne l'interface réseau de LON-SVR2 et clique sur Suivant.

Laisse les paramètres par défaut et clique sur Terminer.

Vérifie que les statuts des deux nœuds sont Convergés.

📌 💻 Étape 5 : Ajouter une nouvelle règle de ports et configurer l'affinité
🔹 Sur LON-SVR2
Ouvre Windows PowerShell et exécute :

powershell
Copier
Modifier
mkdir c:\porttest
xcopy /s c:\inetpub\wwwroot c:\porttest
New-Website –Name PortTest –PhysicalPath “C:\porttest” –Port 5678
New-NetFirewallRule –DisplayName PortTest –Protocol TCP –LocalPort 5678
Ouvre Paint, dessine une ligne sur iisstart.png (dans C:\porttest), enregistre et ferme.

Dans LON-DC1, ouvre Internet Explorer et teste :

Tape http://LON-SVR2:5678 → Vérifie que la page affiche le logo modifié avec la ligne.

🔹 Sur LON-SVR1
Dans Gestionnaire de l'équilibrage de la charge réseau, fais un clic droit sur LON-NLB → Propriétés du cluster.

Onglet Règles de port, supprime la règle existante.

Clique sur Ajouter, entre les valeurs suivantes et clique sur OK :

Plage de ports : 80-80

Protocoles : Les deux

Mode de filtrage : Plusieurs hôtes

Affinité : Aucune

Clique sur Ajouter et entre les valeurs suivantes :

Plage de ports : 5678-5678

Protocoles : Les deux

Mode de filtrage : Un seul hôte

Valide et ferme la fenêtre.

📌 💻 Étape 6 : Valider la configuration
🔹 Sur LON-DC1
Ouvre Internet Explorer.

Tape http://LON-NLB et actualise 20 fois.

Les deux versions du logo doivent apparaître aléatoirement.

Tape http://LON-NLB:5678 et actualise 20 fois.

Seule la version avec la ligne doit apparaître.

📌 💻 Étape 7 : Tester la tolérance aux pannes
🔹 Simuler la panne d’un hôte
Sur LON-SVR1, dans Gestionnaire de l'équilibrage de la charge réseau, fais un clic droit sur LON-SVR1 → Contrôle de l'hôte → Suspendre.

Sur LON-DC1, actualise http://LON-NLB 20 fois et vérifie que seule la version de LON-SVR2 s'affiche.

Sur LON-SVR1, fais Contrôle de l'hôte → Reprendre et vérifie que les deux serveurs reprennent du service.

📌 💻 Étape 8 : Valider la haute disponibilité
🔹 Redémarrer un hôte et vérifier la disponibilité
Sur LON-SVR1, exécute :

powershell
Copier
Modifier
restart-computer
Sur LON-DC1, actualise http://LON-NLB 20 fois et vérifie que le site reste accessible.

🔹 Utiliser Drainstop pour une maintenance sans coupure
Sur LON-SVR2, fais Contrôle de l'hôte → Drainstop dans Gestionnaire de l'équilibrage de la charge réseau.

Sur LON-DC1, actualise http://LON-NLB 20 fois et vérifie que seul le site de LON-SVR1 s'affiche.

🎯 📌 Récapitulatif
✅ Installation de l'équilibrage de charge.
✅ Création du cluster et ajout des hôtes.
✅ Configuration des règles de ports et de l'affinité.
✅ Vérification de la répartition de charge.
✅ Tests de tolérance aux pannes et de haute disponibilité.

Tout est prêt et fonctionnel ! 🎉 Dis-moi si tu veux que j'affine certaines étapes. 🚀
