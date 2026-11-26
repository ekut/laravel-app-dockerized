# laravel-app-dockerized

> [!NOTE]
> Archived. Companion code for a YouTube video. The setup dates from August 2023 and is not maintained, so the image
> versions are those of 2023.

Docker Compose setup for developing a Laravel application without installing PHP, Composer or MySQL on the host. The
application code lives in `src/`.

## Services

| Service    | Image                                 | Purpose                                                  |
|------------|---------------------------------------|----------------------------------------------------------|
| `nginx`    | `nginx:stable-alpine`                 | Web server on http://localhost:8000                      |
| `php`      | `php:8.2-fpm-alpine` with `pdo_mysql` | PHP-FPM behind nginx                                     |
| `mysql`    | `mysql:8.0`                           | Database on host port 3316, data in the `db_data` volume |
| `composer` | `composer:latest`                     | Utility container that runs Composer in `src/`           |
| `artisan`  | same build as `php`                   | Utility container that runs `php artisan`                |

## Usage

Create a Laravel project in `src/`:

```bash
docker compose run --rm composer create-project laravel/laravel .
```

Start nginx, PHP and MySQL:

```bash
docker compose up -d nginx
```

Run Artisan commands:

```bash
docker compose run --rm artisan migrate
```

In the Laravel `.env`, set `DB_HOST=mysql` and `DB_PORT=3306`. The database name, user and password are in
`env/mysql.env`.
