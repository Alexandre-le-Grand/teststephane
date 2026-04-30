# §10 — Matrice de traçabilité

> Document produit dans le cadre du TP 1.6 — Exigences transverses & Matrice de traçabilité SupEvents.  
> Auteur : THOMAS Alexandre  
> Projet : SupEvents — plateforme de gestion et d'inscription aux événements étudiants.

---

## Introduction méthodologique

La présente matrice de traçabilité croise l'intégralité des 21 exigences du cahier des charges SupEvents — 13 exigences fonctionnelles (EF 01 à EF 13) et 8 exigences non-fonctionnelles (ENF 01 à ENF 08) — avec les éléments de la Documentation de Conception Technique (DCT) produits dans les TP précédents. L'objectif est de garantir qu'aucune exigence du CDC n'est orpheline, c'est-à-dire sans module, section de DCT ou ADR associé, et de rendre visibles les zones encore non couvertes à ce stade du projet. Une exigence est considérée comme couverte si au moins un module de §7 ou une section de la DCT en documente la conception ou l'implémentation. Une exigence partiellement couverte ou dont la conception n'est pas finalisée est signalée explicitement pour traitement lors du TP 1.8.

---

## §10.1 — Matrice principale

| Réf. | Intitulé (court) | Module(s) | Section(s) DCT | ADR |
|------|-----------------|-----------|----------------|-----|
| **EF 01** | Création d'événements | EventModule (§7.1) | §6.4 (entité Event), §7.1, §8 (POST /events) | ADR-003 |
| **EF 02** | Catalogue public paginé | EventModule (§7.1) | §6.4 (entité Event), §7.1, §8 (GET /events), §9.2 (PERF-01) | ADR-003 |
| **EF 03** | Fiche détail événement | EventModule (§7.1) | §6.4 (entité Event), §7.1, §8 (GET /events/{id}) | ADR-003 |
| **EF 04** | Inscription payante | TicketModule (§7.3), PaymentModule (§7.4) | §6.4 (entités Ticket, Payment), §7.3, §7.4, §8 (POST /tickets), §9.1.1 (STRIDE flux 1) | ADR-001, ADR-002 |
| **EF 05** | Authentification SSO | AuthModule (§7.2) | §6.4 (entité User), §7.2, §8 (POST /auth/login, /auth/refresh), §9.1 | ADR-004 |
| **EF 06** | Gestion de profil utilisateur | AuthModule (§7.2), UserModule (§7.5) | §6.4 (entité User), §7.2, §7.5, §8 (GET/PUT /users/{id}), §9.3 (RGPD) | — |
| **EF 07** | Confirmation ticket par email | TicketModule (§7.3), NotificationModule (§7.6) | §6.4 (entité Ticket), §7.3, §7.6 | ADR-005 |
| **EF 08** | Annulation d'inscription | TicketModule (§7.3), PaymentModule (§7.4) | §6.4 (entité Ticket), §7.3, §7.4, §8 (DELETE /tickets/{id}) | ADR-002 |
| **EF 09** | Remboursement Stripe | PaymentModule (§7.4) | §6.4 (entité Payment), §7.4, §8 (POST /payments/{id}/refund) | ADR-001 |
| **EF 10** | Tableau de bord organisateur | OrganizerModule (§7.7) | §6.4, §7.7, §8 (GET /organizers/{id}/dashboard) | — |
| **EF 11** | Réception webhook Stripe | PaymentModule (§7.4) | §7.4, §8 (POST /webhooks/stripe), §9.1.2 (STRIDE flux 2) | ADR-001 |
| **EF 12** | Export CSV des inscrits | OrganizerModule (§7.7) | §7.7, §8 (GET /organizers/{id}/attendees/export) | ⚠ Non couvert — à traiter en TP 1.8 |
| **EF 13** | Recherche et filtrage catalogue | EventModule (§7.1) | §6.4, §7.1, §8 (GET /events?q=&category=&date=), §9.2 (PERF-01, index) | ADR-003 |
| **ENF 01** | Performance p95 ≤ 500 ms | EventModule (§7.1), TicketModule (§7.3) | §9.2 (PERF-01, PERF-03) | ADR-006 |
| **ENF 02** | Disponibilité 99,5 % | Tous les modules | §9.2 (PERF-02) | ADR-007 |
| **ENF 03** | Sécurité des paiements (PCI-DSS) | PaymentModule (§7.4) | §9.1.1 (STRIDE flux 1), §9.1.2 (STRIDE flux 2) | ADR-001 |
| **ENF 04** | Protection des données (RGPD) | AuthModule (§7.2), UserModule (§7.5) | §9.3 (registre, mécanismes, procédures) | ADR-008 |
| **ENF 05** | Résistance à la sur-vente | TicketModule (§7.3) | §7.3 (SELECT FOR UPDATE), §9.2 (PERF-03), §9.1.1 (STRIDE-D) | ADR-002 |
| **ENF 06** | Traçabilité des actions (audit log) | Tous les modules | §9.1 (STRIDE-R), §9.3 (audit log inviolable) | ADR-008 |
| **ENF 07** | Scalabilité horizontale | TicketModule (§7.3), PaymentModule (§7.4) | §9.2 (PERF-03, autoscaling) | ADR-007 |
| **ENF 08** | Temps de déploiement continu | Infrastructure DevOps | ⚠ Non couvert — à traiter en TP 1.8 (section §11 — Infrastructure & CI/CD) | — |

---

## §10.2 — Synthèse

### Couverture globale

- **Couverture EF : 12 / 13** — EF 12 (Export CSV des inscrits) non couverte à ce stade (module conçu, endpoint manquant dans §8).
- **Couverture ENF : 7 / 8** — ENF 08 (CI/CD) non couverte (hors périmètre des TP 1.1 à 1.6, traitée en TP 1.8).

### Exigences non couvertes — liste explicite

| Réf. | Intitulé | Statut | Traitement prévu |
|------|----------|--------|-----------------|
| **EF 12** | Export CSV des inscrits | Partiellement couverte — OrganizerModule conçu en §7.7, mais l'endpoint `GET /organizers/{id}/attendees/export` n'est pas documenté dans §8 (section API) | TP 1.8 — documenter l'endpoint dans §8, ajouter le format CSV et les contraintes d'autorisation (accès réservé à l'organisateur de l'événement) |
| **ENF 08** | Déploiement continu | Non couverte — la section §11 (Infrastructure & CI/CD) n'a pas encore été produite | TP 1.8 — produire §11 avec pipeline CI/CD (GitHub Actions), stratégie de déploiement blue-green, et politique de rollback |

### Lecture par colonne — vérification de complétude des modules

| Module | Nb. de lignes dans la matrice | Analyse |
|--------|------------------------------|---------|
| EventModule (§7.1) | 4 lignes (EF 01, 02, 03, 13) | ✅ Cohérent — responsabilité unique : gestion du catalogue |
| AuthModule (§7.2) | 2 lignes (EF 05, 06) + ENF 04, 06 | ✅ Cohérent — responsabilité unique : authentification et identité |
| TicketModule (§7.3) | 4 lignes (EF 04, 07, 08, ENF 05) + PERF-03 | ✅ Cohérent — responsabilité unique : cycle de vie des tickets |
| PaymentModule (§7.4) | 4 lignes (EF 04, 08, 09, 11) + ENF 03 | ✅ Cohérent — responsabilité unique : orchestration des paiements |
| UserModule (§7.5) | 1 ligne (EF 06) + ENF 04 | ✅ Acceptable — module léger de gestion du profil utilisateur |
| NotificationModule (§7.6) | 1 ligne (EF 07) | ⚠ Suspect — module n'apparaissant que sur une ligne. Justification : module transverse de notification (email/push), son périmètre dépasse EF 07 mais les autres notifications (rappels d'événement, alertes de liste d'attente) ne sont pas encore formalisées dans les EF du CDC actuel. À enrichir en TP 1.8. |
| OrganizerModule (§7.7) | 2 lignes (EF 10, 12) | ✅ Cohérent — responsabilité unique : outils pour les organisateurs |
