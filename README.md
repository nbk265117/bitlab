## BetLab Sports Betting Platform

Production-ready Laravel 11 application for operating a sports betting platform with user onboarding, betting slips, deposits/withdrawals, and an admin dashboard. This repository includes a full Docker setup for local development.

### Features
- User auth, KYC checks, password reset
- Admin dashboard for events, markets, outcomes, gateways, and reports
- Multiple payment gateways (cards, crypto, wallets) via configurable drivers
- Notifications via email, SMS, and push
- REST endpoints for deposits (IPN routes) and webhooks

### Tech Stack
- PHP 8.3, Laravel 11
- MySQL 8
- Nginx (Docker)

---

## Quick Start (Docker)

Prerequisites:
- Docker Desktop (or Docker Engine) and Docker Compose

Steps:
1) Clone the repo
```bash
git clone https://github.com/nbk265117/bitlab.git
cd bitlab
```

2) Start containers
```bash
docker compose up -d --build
```

3) Seed environment and app key (first time only)
```bash
docker exec -it betlab-app bash -lc 'cd core && [ -f .env ] || cp .env.example .env || true && php artisan key:generate && php artisan optimize:clear'
```

4) Run migrations (and optionally seed)
```bash
docker exec -it betlab-app bash -lc 'cd core && php artisan migrate --force'
# Optional: php artisan db:seed
```

5) Open the app
- Frontend: `http://localhost`
- Admin: `http://localhost/admin`

If you see a maintenance page, ensure the database is set up and caches are cleared:
```bash
docker exec -it betlab-app bash -lc 'cd core && php artisan optimize:clear'
```

---

## Manual Setup (without Docker)

Prerequisites:
- PHP 8.3 with extensions: BCMath, Ctype, Fileinfo, JSON, Mbstring, OpenSSL, PDO, Tokenizer, XML, cURL, GD
- Composer
- MySQL 8 (or compatible)

Steps:
1) Copy env and set credentials
```bash
cd core
cp .env.example .env
# Edit .env and set DB_* values
```

2) Install dependencies
```bash
composer install --no-interaction --prefer-dist
```

3) Generate app key and migrate
```bash
php artisan key:generate
php artisan migrate --force
php artisan optimize:clear
```

4) Serve (dev only)
```bash
php artisan serve --host=0.0.0.0 --port=8000
# Open http://127.0.0.1:8000
```

Permissions (Linux/macOS):
```bash
chmod -R ug+rw core/storage core/bootstrap/cache
```

---

## Configuration
- App settings are stored in `core/.env` and `core/config/*`.
- Payment gateways can be configured in the admin panel and via database records.
- Queue: set `QUEUE_CONNECTION=database` and run a worker in production.

---

## Useful Commands
```bash
# Inside the PHP container
docker exec -it betlab-app bash

# Clear and rebuild caches
php core/artisan optimize:clear

# Run migrations
php core/artisan migrate --force

# Tail Laravel log
docker exec -it betlab-app bash -lc 'tail -f core/storage/logs/laravel.log'
```

---

## Troubleshooting
- MissingAppKeyException: run `php artisan key:generate` (or the docker exec variant above).
- Database connection errors: verify `DB_*` in `core/.env` matches Docker compose (host `db`, user `betlab`, password `betlab`).
- Stale redirects or views: run `php artisan optimize:clear`.

---

## Security Notice
Do not commit secrets to the repository. Configure API keys and credentials via environment variables (`core/.env`) and the admin panel.


