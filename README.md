# Skill Claude — Accessibilité éditoriale

Un skill pour [Claude.ai](https://claude.ai) qui audite et corrige les erreurs d'accessibilité éditoriale dans vos contenus web.

Proposé par [Murielle Germain](https://www.redactionseo.com) — consultante SEO, rédactrice web et conseillère en accessibilité numérique (RGAA).

---

## Ce que fait ce skill

Il analyse un contenu web — article, email, page, document — et identifie les erreurs d'accessibilité qui relèvent de la rédaction, pas du code. C'est-à-dire tout ce qu'un rédacteur peut corriger lui-même, sans intervention d'un développeur.

Il couvre :

- Les textes alternatifs des images (informatifs, décoratifs, contenant du texte)
- La hiérarchie des titres (H1 → H2 → H3 sans sauts de niveau)
- Les intitulés de liens (pas de « cliquez ici »)
- L'attribut `lang` pour signaler les changements de langue dans le texte
- Les majuscules en bloc (certains lecteurs d'écran épellent chaque lettre)
- Les majuscules accentuées (É, À, Ç, Î…)
- L'italique et la justification du texte
- Les emojis (lus à voix haute par les lecteurs d'écran)
- Les acronymes et abréviations (à développer à la première occurrence)
- Les citations (`<blockquote>` et `<q>`)
- Les tableaux de données (en-têtes de colonnes et de lignes)
- La lisibilité générale (phrases courtes, langage simple, forme active)
- Les liens vers fichiers téléchargeables (format, poids, nouvel onglet)
- Les couleurs (correction éditoriale si l'information dépend uniquement de la couleur ; signalement au développeur pour les problèmes de contraste)

---

## Ce que le skill produit

Selon ce que vous lui demandez :

**Audit** : liste des erreurs trouvées par catégorie, avec la correction proposée.

**Correction** : le texte corrigé directement, avec un résumé des modifications et une note sur ce qui nécessite une intervention HTML ou CSS.

**Brief pour IA** : un bloc d'instructions à ajouter à un prompt pour que le contenu généré respecte ces pratiques dès la première rédaction.

---

## Installation

1. Téléchargez le fichier `accessibilite-editoriale.skill`
2. Ouvrez [Claude.ai](https://claude.ai)
3. Allez dans **Paramètres** (icône en bas à gauche)
4. Cliquez sur **Skills**
5. Glissez-déposez le fichier `.skill` ou cliquez pour le sélectionner

Le skill est actif immédiatement. Il se déclenche automatiquement quand vous soumettez un texte à auditer ou à corriger pour l'accessibilité.

---

## Exemples d'utilisation

> « Audite ce texte pour l'accessibilité éditoriale. »

> « Corrige les erreurs d'accessibilité dans cet article avant publication. »

> « Génère un brief accessibilité à intégrer dans mon prompt de rédaction. »

> « Vérifie si les textes alternatifs de mes images sont conformes RGAA. »

---

## Niveau d'audit selon le type d'entrée

Le skill adapte son périmètre selon ce que vous lui fournissez.

**Source HTML brut** (fichier PHP, HTML collé, fichier uploadé) → audit complet : balisage `lang`, attributs `alt`, en-têtes de tableau `<th>`, citations `<blockquote>`, langue de la page.

**Texte rendu ou URL** → audit partiel : le skill ne peut pas voir les balises HTML. Il audite uniquement ce qui est visible : acronymes, intitulés de liens, majuscules, lisibilité, emojis. Il ne signale jamais comme absent un `lang` ou un `alt` qu'il ne peut pas vérifier.

Pour un audit complet, fournir le code source HTML de la page.

---

## Référentiel

Ce skill s'appuie sur :

- [RGAA 4.1 — DINUM](https://accessibilite.numerique.gouv.fr/) : référentiel légal français, source d'autorité pour la conformité réglementaire
- [AcceDe Web — Notice d'accessibilité éditoriale](https://www.accede-web.com/notices/editoriale/) : référence la plus directement alignée sur les contributions éditoriales, produite par Atalan, licence CC BY
- [WCAG 2.2 — W3C/WAI](https://www.w3.org/TR/WCAG22/) : norme internationale dont le RGAA est une déclinaison
- [Orange a11y-guidelines](https://a11y-guidelines.orange.com/fr/contenu-et-communication/) : bonnes pratiques illustrées pour les contributeurs
- [FALC — Unapei](https://falc.unapei.org/) : critères de lisibilité cognitive (Facile à Lire et à Comprendre)

---

## Versions

| Version | Date | Modifications |
|---|---|---|
| 1.0 | Mai 2026 | Version initiale |
| 2.0 | Mai 2026 | Distinction audit partiel/complet selon type d'entrée, ajout sources de référence |

---

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.fr) — Vous pouvez utiliser, partager et adapter ce skill librement, à condition de citer la source.
