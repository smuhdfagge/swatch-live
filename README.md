# SWATCH website

The website of Support for Women and Teenage Children (SWATCH), a women-led NGO in Kano, Nigeria. Built with Laravel 13.

## What is included

**Public pages**

- Home, with the programme record, reach figures, thematic areas, recent programmes and news
- About us: history, vision and mission, governance, team, registration details
- Our work: ten thematic areas, each with its own page, plus how SWATCH works
- Programmes: the full programme record and a page for each programme
- Partners: every partner organisation and the work done together
- News, with support for stories written on the site and links to press coverage
- Get involved, with a volunteer application form
- Support our work, with ways to give (bank details appear once you add them)
- Contact, with an enquiry form and map
- Privacy notice, sitemap.xml, custom error pages, and redirects from the old WordPress addresses

**Admin area** at `/admin`

- Dashboard with unread messages and new volunteer applications
- Add, edit and delete news (with photos, drafts and scheduled publishing)
- Add, edit and reorder programmes (with photos and a "feature on home page" switch)
- Read and reply to contact messages
- Review volunteer applications and download them as CSV
- View and export newsletter subscribers
- Change your own name, email and password

Every form is protected against spam with a hidden field and rate limits, and sends an email alert to `SWATCH_NOTIFY_EMAIL`.

## Requirements

- PHP 8.3 or newer, with the `pdo_mysql` (or `pdo_sqlite`), `mbstring`, `openssl`, `fileinfo` and `gd` extensions
- Composer
- MySQL 8 / MariaDB 10.6, or SQLite

No Node.js build step is needed. Styles, scripts and fonts are plain files in `public/`.

## Run it locally

```bash
composer install
cp .env.example .env
php artisan key:generate
touch database/database.sqlite        # or set the DB_* values for MySQL
php artisan migrate --seed
php artisan storage:link
php artisan serve
```

Open http://localhost:8000. Sign in at http://localhost:8000/admin with the `ADMIN_EMAIL` and `ADMIN_PASSWORD` from `.env`, then change the password under **My account**.

Run the tests with `php artisan test`.

## Deploy to a server

1. Upload the code or `git clone` it onto the server.
2. Run `composer install --no-dev --optimize-autoloader`.
3. Copy `.env.example` to `.env` and set:
   - `APP_ENV=production`, `APP_DEBUG=false`, `APP_URL=https://swatch.org.ng`
   - the database settings (`DB_CONNECTION=mysql`, `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`)
   - mail settings (`MAIL_MAILER=smtp` and your SMTP details) so form alerts are delivered
   - a strong `ADMIN_PASSWORD`
4. Run:
   ```bash
   php artisan key:generate
   php artisan migrate --seed --force
   php artisan storage:link
   php artisan config:cache
   php artisan route:cache
   php artisan view:cache
   ```
5. Point the domain's document root to the `public` folder.

**On cPanel shared hosting**, put the project outside `public_html` (for example in `~/swatch`), then either point the domain's document root to `~/swatch/public`, or copy the contents of `public/` into `public_html` and change the two paths in `public_html/index.php` to point to `~/swatch`. If `storage:link` is not allowed, create the symlink from the cPanel terminal: `ln -s ~/swatch/storage/app/public ~/public_html/storage`.

After every update, run `php artisan migrate --force` and the three cache commands again.

## Editing content

- **News and programmes:** use the admin area.
- **Organisation details** (address, phone, emails, registration numbers, vision, mission, team numbers, thematic areas): edit `config/swatch.php`. Contact details, social links and bank details can also be set from `.env`.
- **Logo and colours:** the logo is in `public/images/`. Brand colours are at the top of `public/css/site.css`.

Programme figures come from SWATCH's own records. Please check them, and the "Most recent" period for SHARP Task Order 3, before launch.
