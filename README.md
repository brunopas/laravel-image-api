# LaraImage

Laravel REST API for resizing and cropping images by URL or direct upload. Supports albums, Sanctum token authentication, and Intervention Image. Study project from [How to Build REST API in Laravel](https://www.youtube.com/watch?v=bvvVX9Pny84) by The Codeholic.

## Features

- Resize and crop images via API (by URL or file upload)
- Organize images into albums
- Token-based authentication with Laravel Sanctum
- Versioned API routes (`/api/v1`)
- Breeze-powered dashboard for generating API tokens
- JSON resource responses for albums and image manipulations

## Tech stack

- **Runtime:** PHP 8, Laravel 9
- **Auth:** Laravel Sanctum (API tokens), Laravel Breeze (web UI)
- **Image processing:** Intervention Image
- **Database:** MySQL via Eloquent
- **Frontend (dashboard):** Blade, Tailwind CSS, Alpine.js, Vite

See [composer.json](./composer.json) and [package.json](./package.json) for full dependency lists.

## Requirements

- [PHP](https://www.php.net/) >= 8.0.2 with GD or Imagick extension
- [Composer](https://getcomposer.org/)
- [Node.js](https://nodejs.org/) (for building the dashboard frontend)
- [Git](https://git-scm.com/)
- [MySQL](https://www.mysql.com/) (or change the driver in `config/database.php`)

## Environment variables

Copy `.env.example` to `.env` and fill in the values below.

| Variable | Required | Default |
| --- | --- | --- |
| `DB_DATABASE` | Yes | `laravel_image_api` |
| `DB_USERNAME` | Yes | `root` |
| `DB_PASSWORD` | Yes | — |

All other variables use sensible Laravel defaults for local development.

## Getting started

```bash
git clone https://github.com/brunopas/laravel-image-api.git
cd laravel-image-api

composer install
npm install
cp .env.example .env
php artisan key:generate
```

Create a MySQL database called `laravel_image_api` (or whatever you set in `DB_DATABASE`), then:

```bash
php artisan migrate
php artisan storage:link
php artisan serve
```

In a separate terminal, build the frontend assets:

```bash
npm run dev
```

Open [http://localhost:8000](http://localhost:8000). Register a user, generate an API token from the dashboard, and use it in Postman or Insomnia to call the `/api/v1` endpoints.

## API endpoints

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/api/v1/album` | List albums |
| `POST` | `/api/v1/album` | Create album |
| `GET` | `/api/v1/album/{album}` | Show album |
| `PUT` | `/api/v1/album/{album}` | Update album |
| `DELETE` | `/api/v1/album/{album}` | Delete album |
| `POST` | `/api/v1/image/resize` | Resize an image |
| `GET` | `/api/v1/image` | List manipulations |
| `GET` | `/api/v1/image/{image}` | Show manipulation |
| `DELETE` | `/api/v1/image/{image}` | Delete manipulation |
| `GET` | `/api/v1/image/by-album/{album}` | List manipulations by album |

All endpoints require `Authorization: Bearer <token>`.

## Project structure

```text
laravel-image-api/
├── app/
│   ├── Http/
│   │   ├── Controllers/v1/   # AlbumController, ImageManipulationController
│   │   ├── Requests/         # ResizeImageRequest, Album requests
│   │   └── Resources/v1/     # API resource transformers
│   └── Models/               # User, Album, ImageManipulation
├── config/                   # Laravel config files
├── database/migrations/      # Schema for albums and image_manipulations
├── public/                   # Static assets, uploaded images
├── resources/views/          # Breeze auth + dashboard Blade templates
├── routes/
│   ├── api.php               # Sanctum-protected API routes
│   ├── auth.php              # Breeze auth routes
│   └── web.php               # Dashboard web routes
└── tests/                    # PHPUnit tests
```

## License

MIT. See [LICENSE](./LICENSE).
