---
name: accessibilite-editoriale
description: "Auditer et corriger un contenu web selon les bonnes pratiques d'accessibilité éditoriale (RGAA). Utiliser ce skill dès qu'on demande à vérifier, corriger, améliorer ou rendre accessible un texte, un article, un contenu web, un email ou tout document destiné à être publié. S'applique aussi aux prompts destinés à des IA pour générer du contenu accessible, et aux demandes de checklist ou brief éditorial accessibilité."
---

# Accessibilité éditoriale — Audit et correction

Ce skill audite un contenu web (article, email, page, document) et corrige les erreurs éditoriales d'accessibilité. Il couvre les critères RGAA relevant de la responsabilité des rédacteurs — sans toucher au code.

---

## Ce que couvre ce skill

Uniquement les erreurs que peut corriger un rédacteur, sans intervention technique :

- Textes alternatifs des images
- Hiérarchie des titres
- Intitulés de liens
- Langue de la page et changements de langue inline
- Majuscules, italique, justification, mise en forme
- Tableaux de données
- Lisibilité et langage (dont critères FALC)
- Acronymes et abréviations
- Citations
- Liens vers fichiers téléchargeables
- Emojis et caractères spéciaux
- Majuscules accentuées
- Couleurs (correction éditoriale + signalement technique)

---

## Processus d'audit

### Étape 0 — Identifier le type d'entrée

Avant tout, déterminer ce qui a été fourni :

**Source HTML brut** (code PHP, HTML collé directement, fichier uploadé) → audit complet possible sur tous les critères, y compris le balisage (`lang`, `alt`, `<th>`, `<blockquote>`, etc.).

**Texte rendu ou URL** (copier-coller d'une page, résultat d'un fetch web, texte sans balises) → audit partiel uniquement. Les balises HTML ne sont pas visibles : on ne peut pas conclure qu'un `lang` est absent ou qu'un `alt` manque — ils existent peut-être dans le source. Ne jamais signaler comme erreur ce qui relève du balisage HTML quand le source n'est pas disponible.

Si l'entrée est une URL ou du texte rendu et que l'utilisateur souhaite un audit complet, lui demander de fournir le code source HTML.

Périmètre selon le type d'entrée :

| Critère | Texte rendu / URL | HTML source |
|---|---|---|
| Acronymes non développés | ✓ | ✓ |
| Intitulés de liens | ✓ | ✓ |
| Majuscules en bloc, accentuées | ✓ | ✓ |
| Lisibilité, langage | ✓ | ✓ |
| Emojis | ✓ | ✓ |
| Hiérarchie des titres (apparente) | ✓ | ✓ |
| Attribut `lang` sur mots étrangers | ✗ | ✓ |
| Attribut `alt` des images | ✗ | ✓ |
| En-têtes de tableaux (`<th>`) | ✗ | ✓ |
| Citations (`<blockquote>`, `<q>`) | ✗ | ✓ |
| `lang` de la page (`<html lang>`) | ✗ | ✓ |

---

### Étape 1 — Lire le contenu fourni

Analyser le contenu selon son type (voir étape 0). Ne pas inférer l'état du balisage depuis le texte rendu.

### Étape 2 — Identifier les erreurs par catégorie

Passer en revue uniquement les critères accessibles selon le type d'entrée.

### Étape 3 — Restituer

Présenter :
1. Un rappel du mode d'audit appliqué (complet sur HTML source / partiel sur texte rendu)
2. Un résumé court des erreurs trouvées (prose, pas de liste à puces si moins de 3 points)
3. Le contenu corrigé avec les modifications appliquées
4. Si des corrections nécessitent une intervention technique, signaler clairement ce qui relève du HTML et fournir le code corrigé

---

## Critères à vérifier

### Images — Texte alternatif

**Image informative** : l'attribut `alt` doit décrire le contenu utile de l'image en contexte.
- Mauvais : `alt=""` sur une image informative, `alt="image"`, `alt="photo"`, `alt="illustration"`, répétition du nom du fichier
- Bon : `alt="Graphique montrant une baisse de 12 % du trafic organique entre mars et juin 2025"`

**Image décorative** : l'attribut `alt` doit être vide (`alt=""`), jamais absent.
- Une image décorative avec un alt rempli parasite la lecture du lecteur d'écran

**Image contenant du texte** : tout le texte visible dans l'image doit être retranscrit dans l'alt ou dans le corps de la page.

---

### Titres — Hiérarchie

La structure doit être logique et continue : H1 → H2 → H3. On ne saute pas de niveau.
- Mauvais : H1 suivi d'un H3 parce que "le H3 est plus joli graphiquement"
- Mauvais : plusieurs H1 dans la même page
- Bon : une seule H1 par page, H2 pour les sections principales, H3 pour les sous-sections des H2

Corriger en proposant la structure de titres adaptée.

---

### Liens — Intitulés

Un intitulé de lien doit être compréhensible hors contexte (les utilisateurs de lecteurs d'écran naviguent souvent de lien en lien).
- Mauvais : "cliquez ici", "en savoir plus", "lire la suite", "ici", "voir"
- Bon : "Consulter le rapport annuel 2025 de l'ANSSI", "Télécharger le guide RGAA (PDF, 2,3 Mo)"

**Fichiers téléchargeables** : indiquer dans l'intitulé du lien le format et le poids.
- Bon : `Télécharger notre charte graphique (PDF, 4,1 Mo)`

**Liens ouvrant dans un nouvel onglet** : le signaler dans l'intitulé ou via un attribut `title`.
- Bon : `Consulter le site de l'ANSSI (nouvelle fenêtre)`

---

### Langue — Attribut lang

**Langue de la page** : la balise `<html>` doit avoir un attribut `lang` correct (`lang="fr"` pour une page en français).

**Changements de langue inline** : tout mot ou passage dans une autre langue que celle de la page doit être balisé.
- Mauvais : "Notre newsletter est disponible chaque semaine." (mot anglais sans balisage)
- Bon : `Notre <span lang="en">newsletter</span> est disponible chaque semaine.`

Langues courantes à signaler en contexte français : `en` (anglais), `de` (allemand), `es` (espagnol), `it` (italien), `la` (latin).

---

### Mise en forme — Ce qui gêne la lecture

**Majuscules sur des blocs entiers** : certains lecteurs d'écran épellent les majuscules. À remplacer par des `text-transform: uppercase` en CSS, ou des casses titre.
- Mauvais : `DÉCOUVREZ NOTRE OFFRE SPÉCIALE`
- Bon : `Découvrez notre offre spéciale` (le CSS se charge de la mise en forme visuelle)

**Majuscules accentuées** : É, È, Ê, À, Ç, Î, Ù sont obligatoires. Un lecteur d'écran prononce différemment "évenement" et "événement".
- Mauvais : `EVENEMENT`, `A BIENTOT`
- Bon : `ÉVÉNEMENT`, `À BIENTÔT`

**Italique** : déconseillé pour les passages longs. Difficile à lire pour les personnes dyslexiques. Réserver à des usages typographiques ponctuels (titres d'œuvres, termes techniques en première apparition).

**Justification** : le texte justifié crée des espaces irréguliers entre les mots. Toujours aligner à gauche.

**Caractères non standard** : les "ziguigui" (caractères Unicode fantaisie, lettres stylisées, faux gras Unicode) ne sont pas lus correctement par les lecteurs d'écran. Les éviter.

---

### Emojis

Les emojis sont lus à voix haute par les lecteurs d'écran avec leur description officielle Unicode.
- 🚀 est lu "fusée" ou "rocket"
- Plusieurs emojis identiques en série sont épelés un par un
- Un emoji en début de chaque paragraphe = lecture pénible

Règle : supprimer les emojis purement décoratifs. Conserver uniquement ceux dont la description apporte quelque chose.

---

### Acronymes et abréviations

Développer l'acronyme à sa première apparition dans la page.
- Mauvais : "Le RGAA impose des critères stricts."
- Bon : "Le Référentiel Général d'Amélioration de l'Accessibilité (RGAA) impose des critères stricts."

Pour les occurrences suivantes, utiliser `<abbr title="Référentiel Général d'Amélioration de l'Accessibilité">RGAA</abbr>`.

Pour les acronymes en langue étrangère, combiner `<abbr>` et `lang` : `<abbr lang="en" title="Large Language Models">LLM</abbr>`. Le `lang` indique la prononciation, le `title` fournit le développement.

---

### Citations

Utiliser les balises sémantiques adaptées :
- `<blockquote>` pour une citation longue ou en bloc
- `<q>` pour une citation courte inline

Sans ces balises, le lecteur d'écran ne signale pas qu'il s'agit d'une citation — l'utilisateur ne sait pas que les propos cités ne sont pas de l'auteur.

---

### Tableaux de données

Un tableau de données accessible doit avoir :
- Des en-têtes de colonnes (`<th scope="col">`) et/ou de lignes (`<th scope="row">`) balisés
- Un `<caption>` ou un titre adjacent décrivant le contenu du tableau
- Une structure lisible de gauche à droite et de haut en bas
- Pas de cellules fusionnées sauf si absolument nécessaire
- Pas de tableaux imbriqués

Sans en-têtes balisés, le lecteur d'écran annonce une suite de valeurs sans contexte.

**Tableaux de mise en page** : si un tableau est utilisé pour la mise en page (pratique déconseillée), ajouter `role="presentation"` pour que les lecteurs d'écran l'ignorent.

---

### Langage et lisibilité

Critères généraux (bonnes pratiques rédactionnelles) :
- Phrases courtes : une idée par phrase
- Structure de phrase simple : sujet — verbe — complément
- Éviter le jargon non expliqué
- Éviter les subordonnées multiples
- Présent de préférence, forme active
- Éviter les formulations nominales lourdes ("la réalisation de la mise en œuvre de")

Critères FALC (Facile à Lire et à Comprendre — Unapei) applicables au web :
- Utiliser des mots courants et éviter les synonymes inutiles
- Expliquer les métaphores et expressions idiomatiques
- Une seule idée par paragraphe, un seul message par page
- Compléter le texte par des illustrations si elles aident à comprendre
- Éviter les chiffres romains, préférer les chiffres arabes
- Écrire les nombres en chiffres plutôt qu'en lettres au-delà de dix

---

## Sources de référence

Ce skill s'appuie sur :

- **RGAA 4.1 — DINUM** : https://accessibilite.numerique.gouv.fr/ — référentiel légal français
- **AcceDe Web — Notice éditoriale** : https://www.accede-web.com/notices/editoriale/ — référence pour les contributeurs éditoriaux, licence CC BY
- **WCAG 2.2 — W3C/WAI** : https://www.w3.org/TR/WCAG22/ — norme internationale
- **Orange a11y-guidelines** : https://a11y-guidelines.orange.com/fr/contenu-et-communication/ — bonnes pratiques illustrées
- **FALC — Unapei** : https://falc.unapei.org/ — lisibilité cognitive

---

### Couleurs

**Ce que le rédacteur ou l'auteur du contenu peut corriger directement :**

L'information transmise uniquement par la couleur, sans alternative textuelle. Exemples courants :
- Un graphique dont les courbes sont différenciées par leur couleur sans légende textuelle ni motif distinct → ajouter une légende avec des étiquettes textuelles
- Un tableau avec des lignes colorées (vert = validé, rouge = rejeté) sans autre indicateur → ajouter une colonne "Statut" avec les valeurs textuelles
- Un calendrier où des créneaux sont colorés sans étiquette → ajouter un texte dans chaque créneau
- Une infographie où la couleur est le seul vecteur de sens → ajouter des labels

**Ce que le rédacteur ne peut que signaler (relève du CSS/design) :**

- Contraste insuffisant entre le texte et son fond : ratio minimum 4,5:1 pour le texte courant, 3:1 pour le texte de grande taille. À signaler au développeur ou au designer avec le ratio constaté si possible.
- Liens non soulignés identifiables uniquement par leur couleur dans le corps du texte : à signaler au développeur (la correction est en CSS).

---

## Format de restitution

**Si audit uniquement (pas de contenu à corriger) :**
Lister les catégories problématiques, décrire l'erreur, proposer la correction.

**Si correction demandée :**
1. Résumé des erreurs trouvées (bref, en prose)
2. Contenu corrigé complet
3. Note sur les corrections qui nécessitent une intervention HTML ou CSS

**Si brief ou prompt pour IA :**
Rédiger un bloc d'instructions à ajouter au prompt pour que le contenu généré respecte ces pratiques.
