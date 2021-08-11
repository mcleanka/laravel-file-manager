# Development process

-   Install laravel auth starter kit

    -   composer require laravel/breeze --dev
    -   php artisan breeze:install
    -   npm install
    -   npm run dev

-   Install medialibrary for spatie

    -   composer require "spatie/laravel-medialibrary:^9.0.0"
    -   php artisan vendor:publish --provider="Spatie\MediaLibrary\MediaLibraryServiceProvider" --tag="migrations"
    -   php artisan vendor:publish --provider="Spatie\MediaLibrary\MediaLibraryServiceProvider" --tag="config"
    -   php artisan migrate

-   Install laravel file manager

    -   composer require mcleanka/laravel-file-manager
    -   php artisan vendor:publish --tag=fm-config
    -   php artisan vendor:publish --tag=fm-assets

-   Optimize (optional if done already)

    -   php artisan storage:link
    -   sudo apt install jpegoptim optipng pngquant gifsicle
    -   npm install -g svgo
-   Modify User model to (Don't forget to import the traits / classes)
```php
class User extends Authenticatable implements HasMedia
{
    use InteractsWithMedia, ...MoreTraits;
    ...
}
```
-   Create Custom class (CustomPathGenerator) for media library
```php
namespace App\Classes;

use Spatie\MediaLibrary\MediaCollections\Models\Media;
use Spatie\MediaLibrary\Support\PathGenerator\PathGenerator;

class CustomPathGenerator implements PathGenerator
{
    public function getPath(Media $media): string
    {
        $user = auth()->user();

        return request()->get('path') ?? 'archive' . '/' . $user->name . '/';
    }

    public function getPathForConversions(Media $media): string
    {
        return $this->getPath($media) . 'conversions/';
    }

    public function getPathForResponsiveImages(Media $media): string
    {
        return $this->getPath($media) . 'responsive/';
    }
}
```
-   Override path_generate var in medialibrary config file
```php
'path_generator' => App\Classes\CustomPathGenerator::class,
```
-  Create blade file (files.blade.php) and paste
```blade
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>File Manager</title>

	<link rel="stylesheet" href="{{ asset('css/app.css') }}">

	<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.7.0/css/all.css">
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css">
	<link rel="stylesheet" href="{{ asset('vendor/file-manager/css/file-manager.css') }}">

</head>

<body>
	<div style="height: 500px;">
		<div id="fm"></div>
	</div>
	<script src="{{ asset('vendor/file-manager/js/file-manager.js') }}"></script>
</body>

</html>
```
