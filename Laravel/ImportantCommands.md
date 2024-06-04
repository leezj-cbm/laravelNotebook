# Important CLI commands

## Creating Controller

```
php artisan make:controller <controllerName>
```

Makes a controller that needs request and is a controller for the for selected model
```
php artisan make:controller <controllerName> --resource --model=<modelName>
```

Makes a controller that needs request and is a controller for the for selected model
```
php artisan make:controller <controllerName> --model=<modelName> --requests --resource
```
Note: --requests flag will make the storing and updating function within the controller. This will make the StoreXRequest.php and UpdateXRequest.php within the Requests folder which must be filled up with logic!

## Creating Model

Note: -m means to include migration
```
php artisan make:model <ModelName> -m
```

Note:- there are two flags here, f for factory and m for migration
```
php artisan make:model <ModelName> -fm
```

## Create View

```
php artisan make:view <foldername>.<viewname>
```

## Create Resource

```
php artisan make:resource <ResourceName>
```
## Create Storage

Note: create storage for image in tutorial link
```
php artisan storage:link
```

## Migrations

Create a pivot table : example User and Property
```
php artisan make:migration create_property_user_table --create=property_user
```

Start migration

```
php artisan migrate
```

This also applies seed with migration
```
php artisan migrate --seed 
```

This will roll back and then seed
```
Php artisan migrate:refresh --seed 
```

## Enable or disable maintenance mode

```
php artisan down
```

```
php artisan up
```


## Install breeze
```
composer require laravel/breeze --dev
```

```
php artisan breeze:install
```

## Run vite

```
npm run dev
```

## Run tinker

```
php artisan tinker
```


## View all of the routes list:-

```
php artisan route:list
```

## Passport Commands

https://laravel.com/docs/11.x/passport


### Installation

```
php artisan install:api --passport

php artisan passport:keys

```

### Issuing Access Tokens

```
php artisan passport:client



```