# Pharma Delivery MVP

Monorepo d'un MVP pour digitaliser les pharmacies : catalogue, panier, paiement fictif, upload d'ordonnance, et suivi de livraison.

## Dossiers
- backend/ – API REST (Node.js + Express, SQLite pour la démo)
- frontend/ – App web (Next.js style minimal sans dépendances réelles, pages React classiques)

> **Note** : Ce code est un squelette 100% local prêt à être complété. Remplace SQLite par PostgreSQL/MySQL en prod. Ajoute un vrai fournisseur de paiement (Orange Money / MTN MoMo / Stripe), et un service d'email/SMS.

## Lancer en local (exemple rapide)
1) Backend
```bash
cd backend
npm install
npm run dev
```
2) Frontend
```bash
cd ../frontend
npm install
npm run dev
```

## Rôles
- patient (client) – parcourt le catalogue, ajoute au panier, paie, uploade l'ordonnance (si médicament soumis)
- pharmacien – gère le stock, valide les ordonnances, prépare la commande
- livreur – accepte/complète la livraison
- admin – supervision, arbitrage, audit

## Sécurité & Conformité (à adapter à votre pays)
- Vérification d'ordonnance pour médicaments soumis.
- PII/medical privacy : chiffrement en transit (HTTPS), rôles, logs d'audit.
- Traçabilité : historique des statuts de commande.
- Chaîne du froid (si besoin) : flag produit, contrôle à la livraison.
- Clauses légales : CGU, politique de confidentialité, consentement patient.

## Schéma de données (résumé)
- users(id, name, email, phone, role, password_hash)
- pharmacies(id, name, address, phone, lat, lng, is_active)
- medications(id, name, dosage, form, requires_rx, description)
- inventory(id, pharmacy_id, medication_id, price, stock)
- orders(id, user_id, pharmacy_id, status, total_amount, payment_status, created_at)
- order_items(id, order_id, medication_id, qty, unit_price)
- prescriptions(id, order_id, file_path, status, notes)
- deliveries(id, order_id, courier_id, status, eta, address, lat, lng)
- payments(id, order_id, provider, ref, amount, status)
- audit_logs(id, actor_id, action, entity, entity_id, ts, ip)

## Endpoints clés (exemples)
- POST /auth/register, /auth/login
- GET /catalog -> liste + filtres
- POST /cart/checkout -> crée commande (+ prescription si besoin)
- GET /orders/:id
- POST /orders/:id/assign -> livreur
- POST /orders/:id/status -> transitions (admin/pharmacien/livreur)
- POST /prescriptions/:id/review -> pharmacien

## Licence
Usage libre pour démarrage de projet. Pas de garantie. 
