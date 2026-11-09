# Projet : « Mes options après le secondaire »

Page web interactive d'orientation scolaire, destinée à un élève de secondaire 4 et à son parent. En ligne à **https://optionsavenir.chezdario.ca/**.

## État actuel

- La page est dans `docs/index.html` (HTML, CSS et JavaScript en ligne, aucune dépendance, aucun build). Le dossier `docs/` est publié par GitHub Pages (dépôt DarioBoyer/chezdario, branche `main`) : `CNAME` (optionsavenir.chezdario.ca), `.nojekyll` et `404.html` neutre.
- **Ne pas toucher à chezdario.ca** : la racine du domaine garde la page « Coming Soon » de Porkbun, voulue par le propriétaire. Seul le sous-domaine `optionsavenir` pointe vers GitHub (CNAME DNS vers darioboyer.github.io). Aucun lien vers la page nulle part : le propriétaire tape l'adresse directement. La page porte `noindex, nofollow`.
- Aperçu local : `npx http-server@14.1.1 docs -p 8642` puis http://localhost:8642/.
- 54 formations dans le tableau `D` du script (DEP, DEC, AEC, ASP et pistes hors Québec).
- Fonctions en place :
  - graphique cliquable à 5 niveaux de scolarité (sans 3e sec., 3e sec., 4e sec., DES, DES + maths et sciences) ;
  - état par formation : ouvert, possible en concomitance, possible à 18 ans avec le TDG, fermé ;
  - filtres : recherche, diplôme, durée, lieu, voyage, potentiel entrepreneurial, domaine ;
  - intérêt à 3 niveaux et élimination par formation, enregistrés dans `localStorage` (clé `options-formations-v1`) ;
  - bouton « Copier mon résumé ».
- Vérifié : le script s'exécute sous Node avec un DOM simulé et les décomptes par niveau sont justes (1 / 22+16 / 39 / 44 / 54).
- Vérifié le 9 novembre 2026 dans un navigateur : bureau, téléphone (375 px), modes clair et sombre, enregistrement des choix, recherche, TDG. Corrigé : barres du graphique désalignées sur téléphone, débordement des cartes sous 340 px ; ajouté : contour de focus au clavier, mise en page d'impression.
- Domaine : enregistré chez Porkbun (DNS Porkbun). Au 9 novembre 2026, la racine affichait la page « Coming Soon » de Porkbun, sans HTTPS.

## Structure des données

Chaque ligne de `D` :

```
[id, nom, type, domaine, heures, lieu, zone, req, voyage, compte, salaire, perspectives, description, lien, aValider, exigenceTexte]
```

- `heures` : > 0 pour un DEP ou une ASP (mois = heures / 105) ; 0 = DEC de 3 ans ; négatif = durée en mois.
- `zone` : `pres` (Lévis, Québec, Lotbinière), `region` (Beauce, Thetford, Portneuf), `loin`.
- `req` : `2` = 3e secondaire ; `1` = 4e secondaire (concomitance possible avec la 3e) ; `s4` = 4e secondaire stricte ; `des` ; `desm` = DES + maths et sciences ; `ex` = examen d'entrée.
- `voyage` et `compte` : note de 0 à 3. Ce sont des appréciations, pas des données officielles.
- `aValider` : 1 si le préalable n'a pas été confirmé à la source.

La logique d'accès est dans `acces(f, niveau)` et `tdgOk(f, niveau)`.

## Contraintes à respecter

- **Vie privée** : la page est publique. Ne jamais y inscrire le nom, l'âge, la date de naissance ou la situation scolaire de l'élève.
- **Langue** : interface en français québécois, tutoiement, ton simple pour un jeune de 16 ans.
- **Exactitude** : ne pas inventer de préalables, de salaires ni de dates. Toute donnée non confirmée garde la mention « à valider ».
- **Simplicité** : rester sans framework tant que ce n'est pas nécessaire.

## Sources des données (consultées le 9 novembre 2026)

- Brochure 2026-2027 des programmes de FP, Capitale-Nationale et Chaudière-Appalaches : https://cfplimoilou.cssc.gouv.qc.ca/wp-content/uploads/2026/04/pfp2026liens.pdf
- CCQ, Perspectives professionnelles 2026-2030 et fiches métiers : https://www.ccq.org/fr-CA/Nouvelles/2026/perspectives-professionnelles-2026-2030
- Québec.ca, Explorer des métiers et professions ; parcours et passerelles FP
- SRACQ, portes ouvertes des cégeps : https://www.sracq.qc.ca/intervenant/details_activite.aspx?cat=o
- AdmissionFP : https://www.admissionfp.com/
- Sites des établissements (ÉMOICQ, Formation professionnelle des Navigateurs, Institut maritime du Québec, Cégep Limoilou, Cégep de Thetford)

## Travail à faire

1. ~~Tester dans un navigateur~~ (fait).
2. **Déployer** sur optionsavenir.chezdario.ca : GitHub Pages est prêt ; reste à ajouter le CNAME `optionsavenir` → `darioboyer.github.io` chez Porkbun, puis activer « Enforce HTTPS ».
3. **Valider les préalables marqués « à valider »** : extraction de minerai, forage au diamant, forage et dynamitage, transport par camion, technologie de l'architecture, technologie minérale, plongée professionnelle, Garde côtière, éoliennes, ROV.
4. **Idées non vérifiées à documenter avant ajout** : monteur d'acier, échafaudeur, cordiste, mécanicien d'ascenseur, entretien d'aéronefs, pilote, chef de train, foreur pétrolier, guide d'aventure.

## Évolutions possibles

- Partager les choix de l'élève avec le parent (aujourd'hui limités au navigateur de l'élève) : lien encodé dans l'URL, ou petit backend.
- Sortir les données dans un fichier `formations.json` pour les mettre à jour sans toucher au code.
- Ajouter un calendrier des portes ouvertes (dates fermes jusqu'au 6 février 2027).
- Ajouter une vue « comparer deux ou trois formations ».
