## Rapport — C3, M3 et M3_exceptions (16 avril 2026)

---

### C3 — Contexte des réservations

C3 étend C0 et introduit tout ce qui concerne les réservations. Il définit un ensemble `ETAT_RESERVATION` avec deux valeurs distinctes : `valide` et `invalide`, dont la distinction est garantie par `axm4 : valide ≠ invalide`.

La fonction `reservation` associe à chaque véhicule de l'ensemble `VEHICULE` son état de réservation. C'est une fonction totale `VEHICULE → ETAT_RESERVATION`, ce qui signifie que tout véhicule a obligatoirement un état de réservation défini. Les constantes `valide` et `invalide` sont explicitement typées via `axm2` et `axm3` pour satisfaire le vérificateur de types de Rodin.

---

### M3 — Réservations et permissions

M3 raffine M0 et voit C3. Il hérite des trois variables de M0 (`places_pont1`, `places_pont2`, `places_pont3`) et introduit une nouvelle variable `permission_envoyee ∈ BOOL`, initialisée à `FALSE`.

Les événements `monter_pont1`, `monter_pont2` et `monter_pont3` sont raffinés par rapport à M0 : un véhicule ne peut monter sur un pont que si `permission_envoyee = TRUE`, et après l'embarquement la permission est remise à `FALSE`. Cela modélise le fait que chaque conducteur reçoit une autorisation individuelle avant d'accéder au pont.

Deux nouveaux événements sont introduits. `verification_reservation` vérifie que le véhicule présente une réservation valide et qu'aucune permission n'est déjà en cours, puis envoie la permission en mettant `permission_envoyee` à `TRUE`. `rejet_vehicule` identifie les véhicules avec une réservation invalide sans effectuer d'action — la gestion concrète du rejet est laissée au raffinement suivant.

Les gardes supplémentaires `places_pontX + taille(v) ≥ 0` ont été ajoutées dans chaque événement de montée pour aider le prouveur automatique de Rodin à décharger les proof obligations sur les invariants de borne inférieure.

Toutes les proof obligations sont prouvées automatiquement .

---

### M3_exceptions — Gestion des exceptions

M3_exceptions raffine M3 et voit C3. Il n'introduit aucune nouvelle variable ni invariant — tout est hérité de M3. Tous les événements de M3 sont repris avec le mot-clé `EXTENDED`, signifiant qu'ils sont hérités tels quels sans modification.

Un seul événement nouveau est ajouté : `redirection_vehicule`. Il précise concrètement ce qui se passe pour un véhicule avec réservation invalide — il est redirigé vers la voie latérale. Cet événement raffine implicitement `rejet_vehicule` de M3 en donnant un comportement concret au rejet.

Toutes les proof obligations sont prouvées automatiquement .

---

