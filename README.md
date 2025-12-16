# Core Banking Accounts System API

API System qui expose les opérations sur les comptes bancaires Core Banking via Supabase.

## Description

Cette API fournit un accès standardisé aux comptes bancaires (comptes courants, épargne, etc.) du système Core Banking. Elle suit le pattern API-Led Connectivity comme System API.

## Endpoints

### GET /api/accounts
Récupère la liste des comptes avec filtres optionnels.

**Query Parameters:**
- `customerId` (optional): ID du client propriétaire
- `accountNumber` (optional): Numéro de compte
- `accountType` (optional): Type de compte
- `status` (optional): Statut du compte
- `limit` (optional, default: 100): Nombre de résultats
- `offset` (optional, default: 0): Offset pour pagination

### POST /api/accounts
Crée un nouveau compte bancaire.

**Body:** FinancialAccount object

### GET /api/accounts/{accountId}
Récupère un compte par son ID (inclut les propriétaires).

### PUT /api/accounts/{accountId}
Met à jour un compte existant.

### GET /api/accounts/{accountId}/transactions
Récupère les transactions d'un compte.

**Query Parameters:**
- `startDate` (optional): Date de début
- `endDate` (optional): Date de fin
- `transactionType` (optional): Type de transaction
- `status` (optional): Statut de la transaction
- `limit` (optional, default: 100)
- `offset` (optional, default: 0)

### POST /api/accounts/{accountId}/transactions
Crée une nouvelle transaction pour un compte.

### GET /api/transactions/{transactionId}
Récupère une transaction par son ID.

### PUT /api/transactions/{transactionId}
Met à jour une transaction.

### POST /api/transactions/{transactionId}/dispute
Crée un litige sur une transaction.

## Configuration

Voir README.md de Core Banking Customers System API pour la configuration de base.

### Tables Utilisées

- `accounts`: Table principale des comptes
- `account_owners`: Table de relation client-compte
- `transactions`: Table des transactions

## Démarrage

Identique à Core Banking Customers System API (port 8081).

## Architecture Technique

### Flows Business-Logic

- `get-accounts-business-logic`: Liste des comptes
- `create-account-business-logic`: Création de compte
- `get-account-by-id-business-logic`: Récupération par ID
- `update-account-business-logic`: Mise à jour
- `get-account-transactions-business-logic`: Transactions d'un compte
- `create-transaction-business-logic`: Création de transaction
- `get-transaction-by-id-business-logic`: Transaction par ID
- `update-transaction-business-logic`: Mise à jour transaction
- `create-dispute-business-logic`: Création de litige

## Exemples de Requêtes

### GET /api/accounts

```bash
curl -X GET "http://localhost:8081/api/accounts?customerId=123&limit=10"
```

### POST /api/accounts

```bash
curl -X POST http://localhost:8081/api/accounts \
  -H "Content-Type: application/json" \
  -d '{
    "accountNumber": "ACC001",
    "accountType": "Checking",
    "accountName": "Compte Courant Principal",
    "currency": "USD",
    "balance": 5000.00,
    "status": "Active",
    "owners": [{
      "customerId": "123e4567-e89b-12d3-a456-426614174000",
      "relationshipType": "Primary"
    }]
  }'
```

### POST /api/accounts/{accountId}/transactions

```bash
curl -X POST http://localhost:8081/api/accounts/123e4567-e89b-12d3-a456-426614174000/transactions \
  -H "Content-Type: application/json" \
  -d '{
    "transactionNumber": "TXN001",
    "transactionType": "Debit",
    "amount": -100.00,
    "currency": "USD",
    "description": "Retrait ATM",
    "status": "Posted"
  }'
```

