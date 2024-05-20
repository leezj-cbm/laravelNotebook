# General Notes


## Scrap Book

1. Composer.json - All dependencies are listed here

2. Artisan - command line interface

3. Redis - general purpose cache

4. .env - contains all the local parameters of the environment, eg
    - Db connection
    - Port,
    - Db username
    - Db passowrd
    - Redis info
    - Mail info

5. Migration -powerful tool for managing database schema, define and modify structure of db schema , can create and manage db tables and columns programmatically

6. Factory - a convenient way to generate sample data for testing

7. Seeder - populate the database with initial data

8. A 'resource' in Laravel is actually a DTO in SpringBoot. For example, UserResource in Laravel will be UserDTO in SpringBoot.
   - In the resource, ensure that the value part of the associate array in the same as the migration column value. This can cause problems if it is not the same. Laravel can change from snake_case to camelCase, but it can sometimes not be the same!

9. An 'associative array' in PHP is like a HashMap in Java or Dictionary in Python. You can only access the value of an associative array in PHP through its key, not through an index like an array.



## Laravel Structure

1. App is the source code. Contains the subfolders
    - Console - all commands are kept
    - Events - include all events for the project
    - Exceptions - contains handle.php 
    - Http -
      - <mark>Controllers</mark>
      - Middleware
      - Requests
      - Jobs -maintains activities queued for application
      - Listeners
      - Policies
      - Providers- register for events 
        
2. Bootstrap - Encloses all the application bootstrap script - contains file app.php, initializes scripts necessary for bootstrap
    
3. Config - various configuration  and associated parameters for the smooth functioning of laravel app
    
4. Database - various parameters for database functionalities
   - seeders- contains classes used for unit testing db
   - Migrations - helps in queries for migrating the db in web application
   - factories- generate large number of data records
    
5. Public - root folder which helps initializing the Laravel application
   - .htaccess - this file gives server configuration
   - Js and css - considered asset
   - Index.php - initialization of web application
        
6. Resources -contains files that enhances the web application
   - <mark>CSS - BACKUP BEFORE INSTALL BREEZE!!</mark>
   - <mark>JS</mark>
   - <mark>Routes - web.php -> all routes will be here - BACKUP BEFORE INSTALL BREEZE!</mark>
   - <mark>Views - html files or templates with interact with end users and play  a primary role in MVC architecture</mark>
   - Assets - includes files such as LESS and SCSS, required for styling web app
   - Lang - configuration for localization and internalization
        
7. Storage - stores all the logs and necessary files which are freq needed
   - Contains the files that are called in succession
   - Framework - contains session, cache and views which are frequently called
   - Logs - all exceptions and error logs are tracked
        
8. Tests - all unit test are included, naming convention is camel_case

9. Vendor - includes all the composer dependencies

10. Each "entity" in laravel should have its following unique :-
   - DB : migration file -> converts object to database schema via eloquent ORM
   - DB: factory -> tells laravel how to create the objects into db using faker for testing
   - DB: seeder -> this can be shared
   - Http: Controller - this interacts with the SPA or View
   - Http: Resource - this is the DTO to serialize and desearilize to/from JSON
   - Http: Model - configure all the mapping of the 'entity'
   - Resource: view/pages - index file to view this resource/entity. Can implement CRUD view/pages as well

11. Each 'entity' shares the following files:
   - Routes: web.php - these are where all the routings are
   - Resources: layouts
   - Resources: components
   - DB: seeders



### Packages

1. Breeze- starter kit login package for profile management, authentication

2. Ziggy - a dependency in inertia package, handles routing (check in composer.json)

3. Tinker - a command line interface to interact with laravel
   - \App\Models\Project::count()
   - \App\Models\Task::query()->paginate(5)->all()