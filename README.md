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

-   Optimize

    -   php artisan storage:link
    -   sudo apt install jpegoptim optipng pngquant gifsicle
    -   npm install -g svgo

-   Create Custom class for media library
-   Override path_generate var in medialibrary config file
