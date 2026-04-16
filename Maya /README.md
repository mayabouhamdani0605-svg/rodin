
## Rapport — C0 et M0 (16 avril 2026)

---

### C0 — Contexte de base

C0 définit les types et constantes du système. Il contient deux ensembles : `VEHICULE` (les véhicules individuels) et `TYPE_VEHICULE` (les catégories de véhicules). Les quatre types sont définis : `voiture`, `camion1`, `camion2`, `camion3`, et l'ensemble est explicitement énuméré via `axm7`.

La fonction `taille` associe à chaque type son encombrement en nombre de places voiture :
- `voiture` et `camion1` → 1 place
- `camion2` → 2 places
- `camion3` → 3 places

La capacité maximale de chaque pont est fixée à `N = 20`. Un axiome supplémentaire `axm13` garantit que `taille(v) ∈ ℕ1` pour tout type de véhicule, ce qui aide le prouveur automatique de Rodin.

---

### M0 — Machine abstraite des ponts

M0 modélise l'embarquement des véhicules sur les trois ponts du ferry. Trois variables entières `places_pont1`, `places_pont2`, `places_pont3` représentent le nombre de places occupées sur chaque pont.

**Invariants :** Chaque variable est contrainte par trois invariants séparés : appartenance à `ℤ`, borne supérieure `≤ N`, et borne inférieure `≥ 0`. Ce découpage garantit que les valeurs restent dans l'intervalle `[0, N]` tout en permettant au prouveur automatique de Rodin de décharger toutes les proof obligations.

**Initialisation :** Les trois ponts démarrent vides, toutes les variables à 0.

**Événements :** Trois événements modélisent l'embarquement sur chaque pont, avec une règle de priorité stricte :
- `monter_pont1` : accessible dès qu'il reste de la place
- `monter_pont2` : accessible uniquement si `places_pont1 = N`
- `monter_pont3` : accessible uniquement si `places_pont1 = N` et `places_pont2 = N`

Cette priorité respecte l'énoncé : le pont 1 est rempli en premier, puis le pont 2, puis le pont 3.

**Proof obligations :** Toutes les POs sont prouvées automatiquement.

---

### Prochaine étape

Travail sur **C3** et **M3** — réservations et permissions.
