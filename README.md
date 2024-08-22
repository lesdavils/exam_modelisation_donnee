 # Examen pratique 1 👨‍🎓

# Sujet

L’entreprise `XProd` fabrique et commercialise divers produits. Ils sont identifiés par une référence propre à XProd et on enregistre une désignation (libellé court), un descriptif (libellé long) et un prix de vente catalogue unitaire hors taxes.

Dans la base de données, elle gère deux types de produits :
1. `Produits fabriqués` : Pour ces produits, on enregistre le nombre moyen d’heures de main d’œuvre nécessaires à leur fabrication.
2. `Produits approvisionnés` : Ces produits ne sont pas fabriqués par XProd, mais achetés à un ou plusieurs fournisseurs à un prix d’achat unitaire moyen.

Pour ne pas dépendre d’un fournisseur spécifique, enregistré par sa raison sociale, adresse, etc., l’entreprise a établi une liste de fournisseurs capables de livrer chaque produit approvisionné. Pour un même produit, chaque fournisseur peut avoir sa propre référence et un prix différent.

Lorsque XProd passe une commande à une certaine date à un fournisseur, elle essaie de grouper plusieurs lignes de commande : une par produit dans une certaine quantité avec sa date de livraison prévue, afin de réduire les frais de livraison de la commande et essayer de négocier un prix d’achat unitaire inférieur au prix catalogue du fournisseur.

## Travail à faire

- Créer le dictionnaire de données.
- Créer le Modèle Conceptuel des Données (MCD).
-Concevoir le Modèle Logique des Données (MLD).
- Concevoir le Modèle Physique des Données (MPD).


### Pour commencer voici mon `dictionnaire de données`

>Chaque données est classée par rapport a chaque entité avec son type, sa longueur, et est ce qu'il est élémentaire ou alors calculé (dans certains cas).


![alt text](image-1.png)

### Ensuite voici mon `Modèle Conceptuel des Données`

![alt text](ex1_mcd.png)


> _J'ai du rajouter une relation refletive a la main car on ne pouvais pas l'ajouter sur Jmerise._


### Voici le `Modèle Logique des Données` :

![alt text](image.png)

### La relation : 

```r
Produits (  #ref_xprod,   Désignation,   Déscription,   Prix_unitaire,   Type_produit  )

Fournisseur (  #id_Fournisseur,   nom_fournisseur,   raison_sociale_fournisseur,   adresse_fournisseur,   codepostal_fournisseur,   ville_fournisseur,   pays_fournisseur  )

Commande (  #id_Commande,   Date_Commande,   id_Fournisseur_Fournisseur  )

Approvisionnement ext (  #id_approvisionnement_facture,   prix_ht  )

Lignes de commande (  #id_ligne_commande,   quantité,   prix_ht,   date_livraison,   id_Commande_Commande,   id_approvisionnement_facture_Approvisionnement ext  )

Fournis (  #id_Fournisseur_Fournisseur,    #id_approvisionnement_facture_Approvisionnement ext  )

Approvisionne  (  #id_approvisionnement_facture_Approvisionnement ext,    #ref_xprod_Produits  )
```

### Et pour finir voici le modele physique qui permet d'etre importé dans un GDBDD `(Gestionnaire de base de donnée)` comme **Mysql** que j'utilise. 

```sql
CREATE TABLE Produits (
    ref_xprod INT AUTO_INCREMENT,
    Désignation TEXT,
    Déscription TEXT,
    Prix_unitaire FLOAT,
    Type_produit TEXT,
    PRIMARY KEY (ref_xprod)
) ENGINE=InnoDB;

CREATE TABLE Fournisseur (
    id_Fournisseur INT AUTO_INCREMENT,
    nom_fournisseur TEXT,
    raison_sociale_fournisseur TEXT,
    adresse_fournisseur TEXT,
    codepostal_fournisseur INT,
    ville_fournisseur TEXT,
    pays_fournisseur TEXT,
    PRIMARY KEY (id_Fournisseur)
) ENGINE=InnoDB;

CREATE TABLE Commande (
    id_Commande INT AUTO_INCREMENT,
    Date_Commande DATE,
    id_Fournisseur_Fournisseur INT,
    PRIMARY KEY (id_Commande),
    FOREIGN KEY (id_Fournisseur_Fournisseur) REFERENCES Fournisseur(id_Fournisseur)
) ENGINE=InnoDB;

CREATE TABLE `Approvisionnement ext` (
    id_approvisionnement_facture INT AUTO_INCREMENT,
    prix_ht FLOAT,
    PRIMARY KEY (id_approvisionnement_facture)
) ENGINE=InnoDB;

CREATE TABLE `Lignes de commande` (
    id_ligne_commande INT AUTO_INCREMENT,
    quantité INT,
    prix_ht FLOAT,
    date_livraison DATE,
    id_Commande_Commande INT,
    id_approvisionnement_facture_Approvisionnement_ext INT,
    PRIMARY KEY (id_ligne_commande),
    FOREIGN KEY (id_Commande_Commande) REFERENCES Commande(id_Commande),
    FOREIGN KEY (id_approvisionnement_facture_Approvisionnement_ext) REFERENCES `Approvisionnement ext`(id_approvisionnement_facture)
) ENGINE=InnoDB;

CREATE TABLE Fournis (
    id_Fournisseur_Fournisseur INT,
    id_approvisionnement_facture_Approvisionnement_ext INT,
    PRIMARY KEY (id_Fournisseur_Fournisseur, id_approvisionnement_facture_Approvisionnement_ext),
    FOREIGN KEY (id_Fournisseur_Fournisseur) REFERENCES Fournisseur(id_Fournisseur),
    FOREIGN KEY (id_approvisionnement_facture_Approvisionnement_ext) REFERENCES `Approvisionnement ext`(id_approvisionnement_facture)
) ENGINE=InnoDB;

CREATE TABLE Approvisionne (
    id_approvisionnement_facture_Approvisionnement_ext INT,
    ref_xprod_Produits INT,
    PRIMARY KEY (id_approvisionnement_facture_Approvisionnement_ext, ref_xprod_Produits),
    FOREIGN KEY (id_approvisionnement_facture_Approvisionnement_ext) REFERENCES `Approvisionnement ext`(id_approvisionnement_facture),
    FOREIGN KEY (ref_xprod_Produits) REFERENCES Produits(ref_xprod)
) ENGINE=InnoDB;
```

# ⛹️‍♂️ Examen pratique 2 ⛹️‍♂️

### Ci-dessous le dictionnaire de données 

![alt text](image-3.png)

### Puis le MCD

![alt text](image-2.png)

### Puis le MLD

![alt text](image-4.png)

### Le modele  relationnel (MLRD)

```r
Sportifs (  #id_sportif,   nom_sportif,   prénom_sportif,   adresse_sportif,   ville_sportif,   code_postal_sportif,   pays_sportif,   numero_tel_sportif,   fax_sportif,   email_sportif,   numero_license_sportif,   nom_club_sportif,   numero_certificat_medical_sportif,   date_certificat_medical_sportif  )

Compétitions (  #id_competition,   libellé_competition,   date_competition,   nombre_epreuves  )

Epreuves (  #id_epreuves,   nom_epreuve,   ville_epreuve,   distance_epreuve,   id_type_Type  )

Inscription compétition (  #numero_dossard_sportif,   id_competition_Compétitions  )

Type (  #id_type,   libellé_type_epreuves,   distance_type_epreuves,   cond_real_type_epreuves  )

Attribuer (  #id_sportif_Sportifs,    #numero_dossard_sportif_Inscription compétition  )

Contenir (  #id_competition_Compétitions,    #id_epreuves_Epreuves  )
```
 
 