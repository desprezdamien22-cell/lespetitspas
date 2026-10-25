# AGENTS.md : Les Petits Pas

Ce fichier est lu automatiquement par l'agent IA au début de chaque session.
Il décrit le projet, les commandes, les conventions et la règle de mise à jour du backlog.

## 1. Description du projet

Application web « Les Petits Pas » pour une crèche associative de 30 enfants (3 mois à 3 ans), répartis en 3 sections : Bébés, Moyens, Grands.

Deux espaces :
- Espace équipe (rôle staff) : liste des enfants, fiche enfant, saisie des événements (repas, sieste, activité, médicament, incident), messagerie reçue.
- Espace parents (rôle parent) : leurs enfants uniquement, timeline du jour, envoi de messages (500 caractères max).

Stack : Next.js (App Router, Server Actions), Supabase (Auth, Postgres, RLS), ShadCN, Tailwind CSS, Resend (emails), Playwright (tests), Vercel (hébergement).

Environnements : local (.env.local), staging (branche staging, Preview Vercel, base lespetitspas), production (branche main, Production Vercel, base lespetitspas-prod).

Routes : /login, /staff, /staff/children/[id], /staff/messages, /parent, /parent/children/[id], /parent/messages/new.

Structure indicative (à adapter à la structure réelle du projet) :
- app/ : routes et layouts
- components/ : composants UI (ShadCN dans components/ui)
- lib/supabase/ : clients Supabase (client, server, admin)
- lib/actions/ : Server Actions (événements, messages, invitations)
- sql/ : schema.sql (structure, trigger, RLS) et seed.sql (données de test, staging uniquement)
- tests/ : tests unitaires et tests Playwright
- docs/ : featurebonus.md
- backlog.md : Product Backlog (source de vérité)

## 2. Commandes

- Installer : pnpm install
- Lancer en local : pnpm dev (http://localhost:3000)
- Tests unitaires : pnpm test
- Tests end-to-end : pnpm exec playwright test
- Build : pnpm build
- Avant toute proposition de changement, lancer pnpm build et pnpm test ; corriger les erreurs avant de conclure.

## 3. Conventions

- Styles uniquement en classes Tailwind dans le JSX/TSX ; aucun fichier CSS séparé.
- Couleurs et polices via les tokens du thème (globals.css, tailwind.config) ; ne jamais écrire une couleur en dur.
- Composants d'interface issus de ShadCN, personnalisés via Tailwind.
- Préfixes responsive sm:, md:, lg: sur tous les écrans.
- Textes d'interface en français.
- Opérations sensibles (insertion d'événement, envoi d'invitation, changement de statut de message, envoi de message) dans des Server Actions.
- La clé SUPABASE_SERVICE_ROLE_KEY n'est utilisée que côté serveur, jamais préfixée NEXT_PUBLIC_.
- Validation du médicament côté client ET côté serveur (autorisation parentale obligatoire, sinon 403 sans écriture).
- Tous les écrans gèrent les états vides et les erreurs réseau (bouton Réessayer, jamais d'écran blanc).
- La RLS est activée sur toutes les tables.
- Pas de temps réel pour le MVP (pas de Realtime Supabase) : actualisation par rechargement.
- Pas de données de test dans les scripts de production.

## 4. Règle de mise à jour du backlog (obligatoire)

À chaque fois qu'une fonctionnalité ou une User Story de backlog.md est commencée ou terminée :
1. Mettre à jour la colonne Statut de la ligne concernée dans backlog.md : « En cours » au démarrage, « Terminé » une fois les critères d'acceptation vérifiés.
2. Ne jamais modifier les autres lignes sans demande explicite.
3. Si une nouvelle fonctionnalité apparaît, l'ajouter à backlog.md avec un nouvel ID (US-47, US-48…), tous les champs renseignés, et le statut « À faire ».
4. Signaler dans la réponse la ligne modifiée et le nouveau statut.

## 5. Éléments à ne jamais modifier

- Les IDs existants du backlog (US-01 à US-46) et leur ordre.
- Les critères d'acceptation d'une ligne, sauf demande explicite de l'utilisateur.
- Les fichiers .env, .env.local et toute valeur de clé ou de secret.
- Le script de structure de la base de production sans validation de l'utilisateur.
- La règle de redirection par rôle et les policies RLS existantes, sans test d'isolation qui passe.
- Le nom du fichier AGENTS.md et celui de backlog.md.
